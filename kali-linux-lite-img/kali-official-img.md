#!/bin/bash
# Install dependencies (debootstrap)
sudo apt-get install debootstrap
# Fetch the latest Kali debootstrap script from git
curl "https://gitlab.com/kalilinux/packages/debootstrap.git;a=blob_plain;f=scripts/kali;hb=HEAD" > kali-debootstrap &&\
sudo debootstrap kali ./kali-root http://http.kali.org/kali ./kali-debootstrap &&\
# Import the Kali image into Docker
sudo tar -C kali-root -c . | sudo docker import - kalilinux/kali &&\
sudo rm -rf ./kali-root &&\
# Test the Kali Docker Image
docker run -t -i kalilinux/kali cat /etc/debian_version &&\
echo "Build OK" || echo "Build failed!"