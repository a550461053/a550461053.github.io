---
title: ddos攻击
date: 2017-12-21 10:40:58
tags: [ddos, 攻击]
---


## trafgen工具
1. trafgen属于netsniff-ng套件，
	- sudo apt install netsniff-ng

2. use:
	- 先配置ddos攻击文件
		+ ACKFlood攻击
		+ UDP fragment攻击
	- 发起攻击：sudo trafgen --cpp --dev eno1 --conf synflood.trafgen --verbose
	- 停止攻击：
		+ pgrep trafgen | xargs kill -s 9
3. 高级：
	- trafgen性能不够的情况下，通过netsniff-ng攻击进行流量回放：
		+ 先捕获synfloog攻击数据包：
			netsniff-ng --in eno1 --out synflood.pcap --silent --verbose --filter 'ether src 00:50:56:ab:a5:3f' 
			trafgen --cpp --dev eno1 --conf synflood.trafgen --verbose
		+ 再重放synflood攻击数据包：
			netsniff-ng --in synflood.pcap --out ens33 --silent --prio-high --verbose
	- 通过tc工具进行流量控制：
		+ 
