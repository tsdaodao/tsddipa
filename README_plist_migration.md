# iOS IPA 在线安装 - plist文件迁移指南

## 改造说明

原来的系统：
- plist文件存储在本地 `tsdd/` 文件夹中
- plist文件中的IPA下载地址指向GitHub releases

改造后的系统：
- plist文件与IPA文件一起上传到GitHub releases
- HTML页面自动从releases中获取plist文件
- 动态生成安装链接

## 改造内容

### 1. HTML文件修改 (`index2.html`)

主要修改了 `checkurlshowbuttonhtml` 函数：
- 新增参数 `releaseAssets` 和 `currentAsset`
- 在同一个release的assets中查找对应的plist文件
- 如果找到plist文件，生成安装链接
- 如果没找到plist文件，显示下载链接

### 2. plist文件准备

需要修改现有plist文件中的IPA下载地址：

**原来的地址格式：**
```xml
<string>https://github.com/tsdaodao/tsddipa/releases/download/v1/TEMU.ipa</string>
```

**新的地址格式（根据实际release版本调整）：**
```xml
<string>https://github.com/tsdaodao/tsddipa/releases/download/v1.0/TEMU.ipa</string>
```

## 使用方法

### 1. 准备文件
1. 修改plist文件中的IPA下载地址，确保指向正确的GitHub releases地址
2. 将plist文件重命名为与IPA文件相同的名称（扩展名不同）
   - 例如：`TEMU.ipa` 对应 `TEMU.plist`

### 2. 上传到GitHub releases
1. 创建新的release
2. 同时上传IPA文件和对应的plist文件
3. 确保文件名匹配（除了扩展名）

### 3. 访问HTML页面
使用URL参数访问页面：
```
https://yourdomain.com/index2.html?owner=tsdaodao&repo=tsddipa&ext=ipa,plist
```

## 文件命名规则

- IPA文件：`应用名称.ipa`
- plist文件：`应用名称.plist`

例如：
- `TEMU.ipa` 和 `TEMU.plist`
- `任推帮.ipa` 和 `任推帮.plist`

## 注意事项

1. **文件名匹配**：plist文件名必须与IPA文件名完全一致（除了扩展名）
2. **HTTPS要求**：iOS要求plist文件必须通过HTTPS访问
3. **版本一致性**：确保plist文件中的版本信息与实际IPA文件一致
4. **Bundle ID**：plist文件中的bundle-identifier必须与IPA文件中的一致

## 示例

参考 `tsdd/TEMU_example.plist` 文件，了解正确的plist文件格式和配置。 