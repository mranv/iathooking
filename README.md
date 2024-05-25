# DLL Injection and Memory Manipulation in Rust

This Rust project demonstrates DLL injection and process memory manipulation using the Windows API. It includes functions to read and write process memory, inject DLLs into remote processes, and parse export tables of loaded modules.

## Prerequisites

- Rust installed on your system.
- A Windows environment.
- `winapi` crate added to your `Cargo.toml`:
  ```toml
  [dependencies]
  winapi = { version = "0.3", features = ["handleapi", "processthreadsapi", "memoryapi", "libloaderapi", "winnt", "winuser", "tlhelp32"] }
  ```

## Project Structure

### Functions

1. **`FillStructureFromArray`**: Copies data from an array into a structure.
2. **`FillStructureFromMemory`**: Reads data from process memory and fills a structure.
3. **`ReadStringFromMemory`**: Reads a null-terminated string from process memory.
4. **`GetStringFromu8Array`**: Converts a `u8` array to a string.
5. **`GetStringFromi8Array`**: Converts an `i8` array to a string.
6. **`DllInject`**: Injects a DLL into a remote process.
7. **`ParseExports64`**: Parses the export table of a 64-bit PE file in memory.

### Structures

- **`IMAGE_DOS_HEADER`**: Represents the DOS header of a PE file.
- **`IMAGE_NT_HEADERS64`**: Represents the NT headers of a 64-bit PE file.
- **`IMAGE_IMPORT_DESCRIPTOR`**: Represents the import descriptor of a PE file.
- **`IMAGE_EXPORT_DIRECTORY`**: Represents the export directory of a PE file.

## Usage

1. **Clone the repository**:
   ```bash
   git clone https://github.com/mranv/iathooking.git
   cd iathooking
   ```

2. **Build the project**:
   ```bash
   cargo build --release
   ```

3. **Run the project**:
   Modify the `main` function to target the desired process ID and DLL path.
   ```rust
   fn main() {
       let pid: u32 = 15108; // Replace with target process ID
       let loadeddll = "tempdll.dll";

       unsafe {
           let prochandle = OpenProcess(0x001FFFFF, 0, pid);
           if prochandle.is_null() {
               panic!("OpenProcess failed with error: {}", GetLastError());
           }

           DllInject(prochandle, r#"D:\rust_practice\dlls\tempdll\target\release\tempdll.dll"#);

           // Further operations to manipulate memory and parse export tables.
       }
   }
   ```

4. **Run the executable**:
   ```bash
   cargo run --release
   ```

## Important Notes

- This code involves manipulating process memory and injecting DLLs, which can be considered malicious behavior. Ensure you have permission to interact with the target process and understand the legal implications.
- The functions provided are for educational purposes and should be used responsibly.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributions

Contributions are welcome! Please fork the repository and submit a pull request with your changes.
