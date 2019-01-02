# 软件版本

系统 centos

python3.6

​	request

​	selenium

Google Chrome 71.0.3578.98

ChromeDriver 2.43.600233 (523efee95e3d68b8719b3a1c83051aa63aa6b10d)

# 安装python

安装python

yum install epel-release
curl 'https://setup.ius.io/' -o setup-ius.sh
sh setup-ius.sh
yum --enablerepo=ius info python36u
yum --enablerepo=ius install python36u
python3.6 --version


curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3.6 get-pip.py

# 安装chrome

cat << EOF > /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=1
gpgkey=https://dl.google.com/linux/linux_signing_key.pub
EOF

yum install google-chrome-stable

# 安装selenium

pip3.6 install selenium

# 安装requests

pip3.6 install requests

# 安装chromedriver

http://chromedriver.chromium.org/downloads
mv chromedriver /usr/local/share/
ln -s  /usr/local/share/chromedriver /usr/bin/chromedriver