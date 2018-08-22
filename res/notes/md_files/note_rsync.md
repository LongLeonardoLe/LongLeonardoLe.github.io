# How Rsync works
  - generator process: identifies changed files and manages the file level logic
  - sender: the Rsync process that has access to the source files being synchronized
  - receiver: as a role is the destination system; as a process is that receives update data and writes it to disk

### The Pipeline
A set of processes that communicate in a **unidirectional** way. Once th file has been shared the pipleline behaves like:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;generator → sender → receiver

Each process runs independently and is delayed only when the pipelines stall or when waiting for disk I/O or CPU resources.

### The Generator
The process compares the file list with its local directory tree. Each file will be checked to see if it can be skipped:
  - In the most common mode of operation files are **not** skipped if the *modification time* and *size* differs.
  - *Directories*, *device nodes*, and *symlinks* are **not** skipped.
  - Missing directories will be created.

If a file can be skipped, any existing version on the *receiving* side becomes the "basis file" for the transfer, is used to eliminate matching data from having to sent. Block checksums are created for the basis file → sent to the sender following the file's index number

### The Sender
Reads the file index numbers and associated block checksum sets

