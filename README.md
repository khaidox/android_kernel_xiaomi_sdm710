# Custom LineageOS 22.2 Performance Kernel for Xiaomi Mi CC9 / Mi 9 Lite (pyxis)

Mã nguồn Kernel Android 4.9 cho **Xiaomi Mi CC9 / Mi 9 Lite (pyxis)** phiên bản Performance chuẩn cho LineageOS 22.2.

---

## 📌 1. Thông Tin Nhánh Chuẩn (`lineage-22.2`)

- **Repository:** `khaidox/android_kernel_xiaomi_sdm710`
- **Branch:** `lineage-22.2`
- **Compiler:** Clang r416183b + GCC 4.9 (aarch64 & arm 32-bit)
- **LOCALVERSION:** `-okok-perf-${SHORT_SHA}`
- **Release Tag:** `v-lineage-22.2`
- **File nén cài đặt:** `AnyKernel3-pyxis.zip` & `new_boot.img`

---

## 🛠️ 2. Biên Dịch Thủ Công (Local Build)

### 2.1. Cấu Hình Kernel
```bash
make ARCH=arm64 sdm670-perf_defconfig xiaomi/sdm710-common.config xiaomi/pyxis.config
SHORT_SHA=$(git rev-parse --short=8 HEAD)
./scripts/config --file .config --set-str LOCALVERSION "-okok-perf-${SHORT_SHA}"
make ARCH=arm64 CC=clang CLANG_TRIPLE=aarch64-linux-gnu- CROSS_COMPILE=aarch64-linux-android- CROSS_COMPILE_ARM32=arm-linux-androideabi- olddefconfig
```

### 2.2. Biên Dịch Kernel Binary
```bash
make -j$(nproc) \
    ARCH=arm64 \
    CC=clang \
    CLANG_TRIPLE=aarch64-linux-gnu- \
    CROSS_COMPILE=aarch64-linux-android- \
    CROSS_COMPILE_ARM32=arm-linux-androideabi- \
    KCFLAGS="-I$(pwd)/drivers/bluetooth -I$(pwd)/drivers/clk/qcom/mdss -I$(pwd)/drivers/gpu/msm -I$(pwd)/drivers/gpu/drm/msm/sde -I$(pwd)/drivers/media/platform/msm/camera/cam_utils -I$(pwd)/drivers/media/platform/msm/camera_v3/cam_utils -I$(pwd)/drivers/soc/qcom -I$(pwd)/drivers/platform/msm/ipa/ipa_clients -I$(pwd)/drivers/platform/msm/ipa/ipa_v3 -I$(pwd)/drivers/usb/gadget" \
    Image Image.gz-dtb
```

---

## ⚡ 3. Tự Động Hóa Biên Dịch Bằng GitHub Actions (CI/CD)

Workflow `.github/workflows/build-kernel.yml` trên nhánh `lineage-22.2` tự động:
1. Đặt `LOCALVERSION="-okok-perf-${SHORT_SHA}"`.
2. Biên dịch target `Image.gz-dtb`.
3. Đóng gói `AnyKernel3-pyxis.zip` và `new_boot.img`.
4. Đẩy sản phẩm lên trang [GitHub Releases](https://github.com/khaidox/android_kernel_xiaomi_sdm710/releases) dưới tag `v-lineage-22.2`.

### Cách chạy CI/CD:
1. Mở [GitHub Actions Workflows](https://github.com/khaidox/android_kernel_xiaomi_sdm710/actions/workflows/build-kernel.yml).
2. Chọn **Build Xiaomi SDM710 Kernel (pyxis)**.
3. Bấm **Run workflow** $\rightarrow$ Chọn nhánh **`lineage-22.2`**.
