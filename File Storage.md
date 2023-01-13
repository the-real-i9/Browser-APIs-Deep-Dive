# File System Access API
This API allows interaction with files on a user's local device, or on a user-accessible network file system. Core functionality of this API includes reading files, writing or saving files, and access to directory structure.

## File {Read/Write}
- FileSystemFileHandle {Read}
    - File to read from
    - **Get it:** `window.showOpenFilePicker()`
- `FileSystemFileHandle` {Save/Write}
    - File to write to
    - **Get it:** `window.showSaveFilePicker()`
    - FileSystemWritableFileStream {WriteStream}
        - To create the stream to write to in the fileHandle
        - **Create it:** `FileSystemFileHandle.createWritable()`
        - **Write to it:** `writableStream.write([blob, buffer])`
        - **Save to disk:** `writableStream.close()`
- FileSystemFileHandle Methods
    - `createSyncAccessHandle()` - available only in WebWorkers for read/write
    - `createWritable()` - returns a stream to write to
    - `getFile()` - returns a file to read from

## Directory {Access & Modification}
- FileSystemDirectoryHandle {FileSystemHandle}
    - A new directory to access
    - **Get it:** `window.showDirectoryPicker() OR workerNavigator.storage.getDirectory() - root`
    - FileSystemDirectoryHandle {subDirectory}
        - A subdirectory to access/create
        - **Get/Create it:** `FileSystemDirectoryHandle.getDirectoryHandle(name, {create: boolean})`
- FileSystemDirectoryHandle Methods
    - `getDirectoryHandle(name, {create: bool})`
    - `getFileHandle(name, {create: bool})`
    - `removeEntry(name, {recursive: bool})`
    - Iterators: `keys()`, `entries()`, `values()`

## Synchronous **Worker** Storage {Background file handling, without showing pickers}
- `FileSystemDirectoryHandle`
    - **Get it:** `navigator.storage.getDirectory()` - root directory
    - From here you can get/create more directoryHandles and fileHandles
- `FileSystemSyncAccessHandle` {Read/Write}
    - **Get it:** `FileSystemFileHandle.createSyncAccessHandle()`
    - Methods: `read(buffer, {at}), write(buffer, {at}), close(), flush(), getSize(), truncate()`
- Storage Manager methods
    - `getDirectory()`
    - `estimate`
    - `persist()`
    - `persisted()`