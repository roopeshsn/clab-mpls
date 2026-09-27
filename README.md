# MPLS

This repo contains lab files to try and test MPLS with FRR

## Getting Started

### Setup Docker

```
sudo apt update
sudo apt install -y docker.io curl ca-certificates jq iproute2 iputils-ping tcpdump
```

```
sudo systemctl enable --now docker
```

```
sudo usermod -aG docker ubuntu
```

```
exit
multipass shell clab-mpls
```

### Enable MPLS kernel modules inside the VM

```
sudo modprobe mpls_router
sudo modprobe mpls_iptunnel
```

```
lsmod | grep mpls
```

Make the modules persistent:
```
cat << 'EOF' | sudo tee /etc/modules-load.d/mpls.conf
mpls_router
mpls_iptunnel
EOF
```

Create /run/netns, which Containerlab may need:
```
sudo mkdir -p /run/netns
```

If modprobe mpls_router fails, install the extra kernel modules package:
```
sudo apt install -y linux-modules-extra-$(uname -r)
sudo modprobe mpls_router
sudo modprobe mpls_iptunnel
```

### Clab

```
sudo apt update
sudo apt install -y curl iputils-ping iproute2 jq
bash -c "$(curl -sL https://get.containerlab.dev)"
```