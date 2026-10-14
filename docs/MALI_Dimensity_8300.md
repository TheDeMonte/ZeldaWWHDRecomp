# Dimensity/Mali Build Notes

## Mali-G615 MC6 (Dimensity 8300U) Optimization

This fork adds support for Mali GPUs (Dimensity/Exynos chipsets) by disabling Adreno-specific hooks.

### Key Changes

1. **WWHD_DISABLE_ADRENO=ON** — Disables libadrenotools (Adreno GPU hooks not needed for Mali)
2. **CPU=dimensity-8300** — Optimizes for Cortex-A715 + Mali-G615 MC6
3. **Vulkan settings** — Already optimized for mobile: low-res AO by default

### Building

```bash
# Build native libraries with Mali optimizations
cd android
CPU=dimensity-8300 ./build_native.sh

# Build APK (requires Android SDK/NDK on build machine)
./gradlew assembleRelease
```

### Runtime Settings (in-game overlay)

For 60 fps on Mali-G615:
- **Resolution**: 1x-2x (Mali scales well)
- **AO Mode**: 0 (off) or 1 (half) — full AO is GPU-heavy
- **Anisotropy**: On (negligible cost)
- **FXAA**: On (cheap post-process)

### Environment Variables (optional)

| Variable | Values | Effect |
|----------|--------|--------|
| WWHD_VK_PRESENT_MODE | mailbox/fifo/immediate | Presentation mode |
| WWHD_AO_MODE | 0/1/2 | AO off/half/full |
| WWHD_ANISO | 0/1 | Anisotropic filtering |
| WWHD_FXAA | 0/1 | FXAA anti-aliasing |

### Tested On

- Dimensity 8300U (Mali-G615 MC6)
- Android 14

### Notes

- Mali GPUs don't need custom Vulkan drivers — use system driver
- AdrenoTools hooks are for Snapdragon only
- Frame interpolation (60/120/240 fps) works on Mali
