# Rockchip One-KVM DTB Audit

本次审计范围以 `build-armbian/armbian-files/common-files/etc/model_database.conf` 中当前受支持的 Rockchip 设备为准。

- 机型条目数：91
- 唯一 DTB 数：91
- 实际二进制改动：33
- 原本已满足 `peripheral`：0

处理原则：

- 优先逐板确认外露 OTG / Type-C 口，再锁定目标 USB 控制器节点。
- 在用户明确允许后，仅对“同 SoC、同 OTG 控制器地址、且只有单一 OTG 节点”的 DTB 做组内扩展。
- 在用户最后明确要求后，将支持集中剩余的 `dr_mode = "otg"` 节点全部收口为 `peripheral`。
- 不按板名或 SoC 名称做模糊猜测；审计基于每个 DTB 的实际 USB 节点结构、目标节点路径和原始 `dr_mode`。

## Topology A: `/usb@fe800000/usb@fe800000`

适用结构：`rk3399 DWC3，主 OTG 在 fe800000，fe900000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3399-aio-3399c.dtb` | `otg` | `peripheral` | 逐板确认的 Type-C/OTG 板，主 OTG 节点改为 peripheral |
| `rk3399-firefly.dtb` | `otg` | `peripheral` | 逐板确认的 Type-C/OTG 板，主 OTG 节点改为 peripheral |
| `rk3399-aio-3399c-ai.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fe900000` 保持 host |
| `rk3399-emb3531.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fe900000` 保持 host |
| `rk3399-kylin3399.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fe900000` 保持 host |
| `rk3399-nanopc-t4.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fe900000` 保持 host |
| `rk3399-taram.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fe900000` 保持 host |

## Topology B: `/usb@fe800000/dwc3`

适用结构：`rk3399 DWC3，主 OTG 在 fe800000，内层节点名为 dwc3`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3399-leez-p710.dtb` | `otg` | `peripheral` | 内层节点名不同，但控制器地址仍是 `fe800000`，另一条 `fe900000/dwc3` 保持 host |

## Topology C: `/usb@fcc00000`

适用结构：`rk3566/rk3568 DWC3，主 OTG 在 fcc00000，fd000000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3566-roc-pc.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`fd000000` 保持 host |
| `rk3568-lyt-t68m.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`fd000000` 保持 host |
| `rk3568-lz-d3568-v3.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`fd000000` 保持 host |
| `rk3568-lz-k3568.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`fd000000` 保持 host |
| `rk3568-roc-pc.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`fd000000` 保持 host |

## Topology D: `/usbdrd/usb@fcc00000`

适用结构：`rk3568 DWC3，主 OTG 在 usbdrd/usb@fcc00000，usbhost/usb@fd000000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3568-gzpeite.dtb` | `otg` | `peripheral` | `usbdrd` 下主 OTG 节点改为 peripheral |
| `rk3568-hlink-h66k.dtb` | `otg` | `peripheral` | `usbdrd` 下主 OTG 节点改为 peripheral |
| `rk3568-hlink-h68k.dtb` | `otg` | `peripheral` | `usbdrd` 下主 OTG 节点改为 peripheral |
| `rk3568-hlink-h69k.dtb` | `otg` | `peripheral` | `usbdrd` 下主 OTG 节点改为 peripheral |
| `rk3568-radxa-e25.dtb` | `otg` | `peripheral` | `usbdrd` 下主 OTG 节点改为 peripheral |

## Topology E: `/usbdrd/dwc3@fcc00000`

适用结构：`rk3568 DWC3，主 OTG 在 usbdrd/dwc3@fcc00000，usbhost/dwc3@fd000000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3568-ruisen-box.dtb` | `otg` | `peripheral` | 内层节点名为 `dwc3@fcc00000`，主 OTG 节点改为 peripheral |

## Topology F: `/usbdrd/dwc3@fe500000`

适用结构：`rk3528 DWC3，主 OTG 在 fe500000`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3528-hlink-h28k.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral |
| `rk3528-radxa-e20c.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral |

## Topology G: `/soc/usb@23000000`

适用结构：`rk3576 DWC3，主 OTG 在 23000000，23400000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3576-nanopi-m5.dtb` | `otg` | `peripheral` | 主 OTG 节点改为 peripheral，`/soc/usb@23400000` 保持 host |

## Topology H: `/usbdrd3_0/usb@fc000000`

适用结构：`rk3588/rk3588s DWC3，主 OTG 在 fc000000，fcd00000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3588-orangepi-5-plus.dtb` | `otg` | `peripheral` | 逐板确认的 Type-C/OTG 板，主 OTG 节点改为 peripheral |
| `rk3588-rock-5b.dtb` | `otg` | `peripheral` | 逐板确认的 Type-C/OTG 板，主 OTG 节点改为 peripheral |
| `rk3588s-orangepi-5b.dtb` | `otg` | `peripheral` | 逐板确认 + 运行时确认，主 OTG 节点改为 peripheral |
| `rk3588-dc-a588.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fcd00000` 保持 host |
| `rk3588-hlink-h88k.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fcd00000` 保持 host |
| `rk3588-hlink-h88k-v31.dtb` | `otg` | `peripheral` | 与同 SoC 主组同路径，`fcd00000` 保持 host |

## Topology I: `/usb@fc000000`

适用结构：`rk3588 DWC3，主 OTG 在 fc000000，节点直接挂在根级`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3588-friendlyelec-cm3588-nas.dtb` | `otg` | `peripheral` | 逐板确认的全功能 USB-C 板，主 OTG 节点改为 peripheral |
| `rk3588-rock-5-itx.dtb` | `otg` | `peripheral` | 逐板确认的 Type-C/OTG 板，主 OTG 节点改为 peripheral |

## Topology J: `/usb@fe900000/usb@fe900000`

适用结构：`rk3399 DWC3，OTG 位于 fe900000，fe800000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3399-firefly-core-3399-jd4.dtb` | `otg` | `peripheral` | 最后一轮收口，OTG 位于 `fe900000`，与 rk3399 主组相反 |

## Topology K: 双 OTG `/usb@fc000000` + `/usb@fc400000`

适用结构：`rk3588 DWC3，两路原始 OTG 节点并存，fcd00000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3588-nanopc-t6.dtb` | `otg` + `otg` | `peripheral` + `peripheral` | 最后一轮收口，两路 OTG 节点一并改为 peripheral |

## Topology L: 双 OTG `/usbdrd3_0/usb@fc000000` + `/usbdrd3_1/usb@fc400000`

适用结构：`rk3588 DWC3，两路 usbdrd 原始 OTG 节点并存，fcd00000 保持 host`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `rk3588-smart-am60.dtb` | `otg` + `otg` | `peripheral` + `peripheral` | 最后一轮收口，两路 OTG 节点一并改为 peripheral |

## 当前结论

- 当前 91 个受支持 Rockchip DTB 中，原始含 `otg` 节点并需要处理的 33 个 DTB 已全部完成修改
- 当前支持集内已无残留 `dr_mode = "otg"` 节点
- 其余未修改 DTB 均不含需要从 `otg` 调整为 `peripheral` 的目标节点
