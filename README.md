# Nro Nox - iOS (Unity 2022.3.62f3 export)

Xcode project export từ Unity cho game **Nro Nox** (`com.skyz.kamui3tab`), dùng để build IPA bằng GitHub Actions.

## Build IPA bằng GitHub Actions

Workflow `.github/workflows/build-ipa.yml` tự chạy mỗi khi push lên `main`, hoặc bấm **Run workflow** trong tab **Actions**.

Mỗi lần build xong sẽ có:

- **Release**: `ipa-<số build>` kèm file `NroNox.ipa` (tải trực tiếp, không cần login)
- **Artifacts**: `NroNox-ipa-<số build>` (lưu 30 ngày)

> IPA được build **unsigned** (`CODE_SIGNING_ALLOWED=NO`). Cần ký bằng Sideloadly / AltStore / ESign / TrollStore trước khi cài lên iPhone.

## Yêu cầu

- Repo dùng **Git LFS** cho các file `.a` (ví dụ `Libraries/libiPhone-lib.a` ~196 MB, vượt giới hạn 100 MB của GitHub).
- Khi clone về máy: cài `git lfs` rồi `git lfs install` trước khi clone.

## Build thủ công trên macOS

```bash
xcodebuild archive \
  -project Unity-iPhone.xcodeproj \
  -scheme Unity-iPhone \
  -configuration Release \
  -sdk iphoneos \
  -destination 'generic/platform=iOS' \
  -archivePath build/NroNox.xcarchive \
  CODE_SIGNING_ALLOWED=NO CODE_SIGNING_REQUIRED=NO CODE_SIGN_IDENTITY=""

mkdir -p build/Payload
cp -R build/NroNox.xcarchive/Products/Applications/*.app build/Payload/
cd build && zip -qry NroNox.ipa Payload
```
