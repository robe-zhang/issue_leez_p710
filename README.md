# Leez P710 BOARD ISSUE：
  
  
![leez_p710.jpg](https://github.com/robe-zhang/issue_leez_p710/blob/master/leez_p710.jpg)
## 问题：
leez 开发板网络卡顿，千兆网口丢包严重。如图：gmac_issue.png  
![gmac_issue.png](https://github.com/robe-zhang/issue_leez_p710/blob/master/gmac_issue.png)
  
硬件：(小批量生产的，无 EMMC)  
  
uboot: mainline 2019.10-rc3  
内核：mainline 4.19.72  
系统：ubuntu 18.04 aarch64 打包时间 20190908  
  
  
## DEBUG：修改 DTS
  
```
kernel/arch/arm64/boot/dts/rockchip/rk3399-p710.dtsi
        assigned-clock-parents = <&clkin_gmac>;
        pinctrl-names = "default";
        pinctrl-0 = <&rgmii_pins>;
-       tx_delay = <0x28>;
-       rx_delay = <0x11>;
+       tx_delay = <0x23>;
+       rx_delay = <0x09>;
        status = "okay";
};
```
  
  
## 测试方法：
  
测试硬件：全千兆路由 + 六类线 + 全千兆主机(dell x86 电脑，khadas edge captain 开发板)  
1，发收 20 个数据包，统计一次丢包率  
2，连续统计 200 次以上，记录丢包率曲线。  


## 测试数据：  
修改前后测试数据：如图 gmac_status.png:   
![gmac_status.png](https://github.com/robe-zhang/issue_leez_p710/blob/master/gmac_status.png)
修改前：丢包率 10% - 70%，平均丢包率 40%。(测试原始数据：gmac_status_log_default_dts )    
修改后：丢包率 0%。(测试原始数据：gmac_status_log_modify_dts )    
  

