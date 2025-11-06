# Rockchip-pcie-devices
Inspired by @geerlingguy/raspberry-pi-pcie-devices for Rockchip SoC's.

### This is a public effort to document PCIe Device support for Rockchip based (Single Board) Computers
**Contributions are Welcome!**

# Status

- **RK3588**: 
  - ✅ "All" used by `amdgpu` driver with this [patch](https://github.com/mariobalanica/arm-pcie-gpu-patches)
  - ✅ Nvidia proprietary driver with the same [patch](https://github.com/mariobalanica/arm-pcie-gpu-patches/issues/2#issuecomment-3478190881)
  - ❌ Nvidia with `nouveau`

---

### My tests are done on:
| SoC  |  Board |
|---|---|
| RK3588  | Radxa Rock 5B / 5B+ / 5T |
| RK3568  | FriendlyElec NanoPi R5S |

### But also open to contributions for:

| SoC  |  Board |
|---|---|
| RK3582  | any |
| RK3576  | any |
| RK3566  | any |
| RK3528  | any |
