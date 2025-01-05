# 处理字符流使用Writer与Reader
- 以 **字符（16 位 Unicode 字符）** 为单位进行数据读写。
- 适用于处理文本数据（如 `.txt`、`.java` 等文件）。
- 主要类是 `Reader` 和 `Writer`。
# 处理字节流-使用OutputStream与InputStream
* 以 **字节（8 位）** 为单位进行数据读写。
- 适用于处理二进制数据（如图片、音频、视频等）或任何类型的文件。
- 主要类是 `InputStream` 和 `OutputStream`
# 对象流类
- 对象流类是用于序列化和反序列化对象的类，例如 `ObjectInputStream` 和 `ObjectOutputStream`。
- `File` 类并不涉及对象的序列化或反序列化，因此它不是对象流类。
# 非流类
- `File` 类的主要功能是表示==文件或目录的路径==，并提供一些操作方法（如创建、删除、重命名、检查文件是否存在等）。
- 它并不直接参与数据的读写操作，因此不属于流类。