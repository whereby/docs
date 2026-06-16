# File Sharing

Share files with everyone in the room. A file is uploaded and then posted to the room as a chat message, so it appears both in `state.fileUploads` (local upload progress for files **you** send) and on the `file` property of the corresponding `state.chatMessages` entry (for every participant, including the sender).

### Methods

#### `sendFiles(files: File[]): void`

Validate, upload, and share the given files with the room. Invalid files are rejected locally (see Validation); valid files are still uploaded. Only one `sendFiles` request can be in flight at a time. Progress and per-file errors are reflected in `getState().fileUploads`.

```ts
const roomConnection = wherebyClient.getRoomConnection();

roomConnection.sendFiles(files); // files: File[]

const unsubscribe = roomConnection.subscribe((state) => {
    for (const upload of state.fileUploads) {
        console.log(upload.name, upload.status, upload.error);
    }
});
```

#### `downloadFile(file: ChatFileShare): Promise<Blob>`

Download a shared file. Resolves with the file contents as a `Blob`, or rejects if the download fails. Use the `file` property from a chat message.

```ts
const blob = await roomConnection.downloadFile(file); // file: ChatFileShare
const url = URL.createObjectURL(blob);
// ...use `url`, then URL.revokeObjectURL(url)
```

### State

| Field                  | Type                         | Description                                                                     |
| ---------------------- | ---------------------------- | ------------------------------------------------------------------------------- |
| `fileUploads`          | `FileUpload[]`               | The files you have shared this session, with per-file upload status.            |
| `chatMessages[n].file` | `ChatFileShare \| undefined` | Present on chat messages that carry a shared file. Pass this to `downloadFile`. |

#### `FileUpload`

```ts
interface FileUpload {
    id: string;
    name: string;
    size: number;
    type: string;
    status: "uploading" | "sent" | "error";
    error?: FileShareError;
}
```

#### `ChatFileShare`

```ts
interface ChatFileShare {
    downloadUrl: string;
    name: string;
    size: number;
    type: string;
    key: string;
    id?: string;
}
```

#### `FileShareError`

```ts
type FileShareError =
    | "file_sharing_not_available" // file sharing is not available for this room
    | "file_sharing_not_enabled"   // file sharing is disabled for this room
    | "not_in_a_room_session"      // no active room session to share into
    | "upload_failed"              // the upload to storage failed
    | "too_many_files"             // more than MAX_FILES_PER_UPLOAD files in one call
    | "unsupported_file_type"      // file type is not in ACCEPTED_FILE_TYPES
    | "file_too_large";            // file exceeds MAX_FILE_SIZE
```

### Validation

`sendFiles` validates files on the client before uploading, mirroring the limits the backend enforces, so failures surface immediately in `fileUploads`:

* **More than `MAX_FILES_PER_UPLOAD` (10) files in a single call** fails the whole batch — every file is marked `status: "error"` with `error: "too_many_files"` and nothing is uploaded.
* Otherwise, each file is checked individually. A file whose type is not in `ACCEPTED_FILE_TYPES` is marked `unsupported_file_type`, and a file larger than `MAX_FILE_SIZE` is marked `file_too_large`. **Rejected files are skipped; the remaining valid files are still uploaded and shared.**

#### Exported constants

Re-exported from `@whereby.com/core` so you can validate before calling `sendFiles`:

```ts
import { MAX_FILES_PER_UPLOAD, MAX_FILE_SIZE, ACCEPTED_FILE_TYPES } from "@whereby.com/core";
```

| Constant               | Value                      | Description                                   |
| ---------------------- | -------------------------- | --------------------------------------------- |
| `MAX_FILES_PER_UPLOAD` | `10`                       | Maximum number of files per `sendFiles` call. |
| `MAX_FILE_SIZE`        | `15728640` (15 MB)         | Maximum size, in bytes, of a single file.     |
| `ACCEPTED_FILE_TYPES`  | `Record<string, string[]>` | Map of accepted MIME type to file extensions. |

**Supported formats:** `.doc`, `.docx`, `.pdf`, `.rtf`, `.xlsx`, `.csv`, `.txt`, `.mp3`, `.wav`, `.gif`, `.jpg`, `.jpeg`, `.png`, `.webp`, `.mp4`, `.mov`, `.webm`, `.mkv`.
