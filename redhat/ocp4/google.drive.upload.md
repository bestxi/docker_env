# google drive upload

https://olivermarshall.net/how-to-upload-a-file-to-google-drive-from-the-command-line/

https://github.com/gdrive-org/gdrive

```bash
############################
## split and merge
split -b 10G registry.tgz registry.
cat registry.?? > registry.tgz

## for cmcc split and merge on osx
split -b 2000m ocp4.tgz ocp4.
split -b 2000m registry.tgz registry.
split -b 2000m rhel-data.tgz rhel-data.


################################
## skicka

yum install -y golang

go get github.com/google/skicka
install /root/go/bin/skicka /usr/local/bin/skicka
skicka init
skicka -no-browser-auth ls

skicka ls "/zhengwan.share/shared_docs/2020.01/ocp.ccn/"
cd /data
mkdir -p /data/upload
/bin/mv -f *.tgz ./upload/
/bin/mv -f registry.* ./upload/
cd /data/upload/

# find ./ -maxdepth 1 -type f -exec skicka upload {}  "/wzh/wangzheng.share/shared_docs/2019.11/ocp 4.2.8/" \;

find ./ -maxdepth 1 -name "*.tgz" -exec skicka upload {}  /"zhengwan.share/shared_docs/2020.01/ocp.ccn/" \;

skicka download "/other.deep.folder/A北区SA资料库/Discovery Session/"

find ./ -maxdepth 1 -name "*.mp4" -exec skicka upload {}  "/zhengwan.share/shared_docs/2020.03/GPTE Advanced Service Mesh/" \;

##################################
## rsync
yum -y install connect-proxy

export VULTR_HOST=nexus.redhat.ren

export VULTR_HOST=zero.pvg.redhat.ren

export VULTR_HOST=vcdn.redhat.ren

export VULTR_HOST=bastion.5311.example.opentlc.com

cat << EOF > /root/.ssh/config
StrictHostKeyChecking no
UserKnownHostsFile=/dev/null

Host *.redhat.ren 
    ProxyCommand connect-proxy -S 192.168.253.1:5085 %h %p
Host *.opentlc.com 
    ProxyCommand connect-proxy -S 192.168.253.1:5085 %h %p
EOF
# ProxyJump user@bastion-host-nickname
# -J user@bastion-host-nickname

# sync from aws to localvm
cd /data

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/ocp4 /data/

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/registry /data/

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/mirror_dir /data/is.samples/

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/mirror_dir/ /data/mirror_dir/

## sync from aws to pvg
cd /data/remote/4.4.7

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/ocp4 ./

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/registry /data/

# copy to local disk
# localvm.md

cd /root
tar -cvf - data/ | pigz -c > /mnt/hgfs/ocp/rhel-data-7.8.tgz

cd /data
tar -cvf - ocp4/ | pigz -c > /mnt/hgfs/ocp/ocp.4.3.21.tgz
tar -cvf - registry/ | pigz -c > /mnt/hgfs/ocp/registry.4.3.21.tgz
tar -cvf - is.samples/ | pigz -c > /mnt/hgfs/ocp/is.samples.4.3.5.tgz

# sync to base-pvg
rsync -e ssh --info=progress2 -P --delete -arz  /root/data ${VULTR_HOST}:/var/ftp/

rsync -e ssh --info=progress2 -P --delete -arz ./mirror_dir ${VULTR_HOST}:/data/remote/4.3.3/is.samples/

# sync from base-pvg
export VULTR_HOST=zero.pvg.redhat.ren

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/var/ftp/data /root/

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/remote/4.3.3/is.samples/mirror_dir ./

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/remote/4.3.21/ocp4 ./

rsync -e ssh --info=progress2 -P --delete -arz ${VULTR_HOST}:/data/remote/4.3.21/registry ./

split -b 5000m ocp.4.3.21.tgz ocp.4.3.21.
split -b 5000m registry.4.3.21.tgz registry.4.3.21.
split -b 5000m rhel-data-7.8.tgz rhel-data-7.8.

# sync to vcdn.redhat.ren
rsync -e ssh --info=progress2 -P --delete -arz  /root/data ${VULTR_HOST}:/data/rhel-data

rsync -e ssh --info=progress2 -P --delete -arz /data/registry ${VULTR_HOST}:/data/

rsync -e ssh --info=progress2 -P --delete -arz /data/ocp4 ${VULTR_HOST}:/data/

rsync -e ssh --info=progress2 -P --delete -arz /data/is.samples ${VULTR_HOST}:/data/




####################
## local mac
# ls -1a *.list

bash
cd /Users/wzh/Desktop/dev/docker_env/redhat/ocp4/files/4.2/image_lists

var_files=$(cat << EOF
operator.failed.list
operator.image.list
operator.ok.list
pull.image.failed.list
pull.image.ok.list
pull.sample.image.failed.list
pull.sample.image.ok.list
yaml.image.ok.list
yaml.sample.image.ok.list
install.image.list.tmp.uniq
yaml.image.ok.list.uniq
EOF
)

while read -r line; do
    scp root@192.168.253.11:/data/ocp4/${line} ./
done <<< "$var_files"

```

gdrive below
```bash
wget https://github.com/gdrive-org/gdrive/releases/download/2.1.0/gdrive-linux-x64

mv gdrive-linux-x64 gdrive
chmod +x gdrive
install gdrive /usr/local/bin/gdrive

# following the link and give back the code
gdrive list

gdrive upload ***.tgz
```
