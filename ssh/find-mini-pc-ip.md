# Find Mini PC IP

For mini PCs that have known IP prefix but unknown IP address, we can use `nmap` to scan the network and find the IP address.

```bash
nmap -sP 192.168.1.0/24
```

**Author**: Vio
**Date**: 2024/07/03
**Home Page**: [Vio's GitHub](https://github.com/coolzz27)
