### Documentation
# This is a Dockerfile for development on slampp
#
##

MAINTAINER Armando Ibarra, armandoibarra1@gmail.com

LABEL version="0.0.1" description="A container for slampp"

# Set color variables to make results and echo statements cleaner 
RED="\e[31m" 
GREEN="\e[32m" 
NC="\e[0m"  #No color 

ENV SC_VERSION 0.0.1

#FROM slampp:2.0.2
CMD "printf" "${GREEN}This is a $SC_VERSION  ${NC}\n"; 

CMD "printf" "${RED}Please wait config enviroment${NC}\n"; 

USER root

#set keyborad layout
RUN setxkbmap es

#show info about slampp
RUN cat /etc/slampp-version
RUN cat /etc/slackware-version
RUN cat /etc/slackware-version-info


#Bits
RUN uname -n
RUN uname -o

#http://www.slackpkg.org/

#First update db 

#backup file
#RUN cp /etc/slackpkg/mirrors /etc/slackpkg/mirrors.bak 
#RUN cp /etc/slackpkg/mirrors /etc/slackpkg/mirrors.old 

COPY /etc/slackpkg/mirrors /etc/slackpkg/mirrors.bak 
COPY /etc/slackpkg/mirrors /etc/slackpkg/mirrors.old 

#Best
#cp /etc/slackpkg/mirrors{,.bak,.old}
#cp /etc/slackpkg/mirrors{,.old}

#Uncomment a mirror 
# grep -E -A 1 ".*slackware-13.0.*$" /etc/slackpkg/mirrors.bak | grep -E -A 1 "\# http://" | \
# sed '/--/d'| sed 's/\#//g' > /etc/slackpkg/mirrors

#cut -d "." -f 2

# OK2=$( grep -E -A 1 ".*slackware-13.0.*$" /etc/slackpkg/mirrors.bak | grep -m 1 "\# http://" | \
# sed '/--/d'| sed 's/\#//g' > /etc/slackpkg/mirrors

#export MIRROR_URL
MIRROR_URL=$( grep -E -A 1 ".*slackware-13.0.*$" /etc/slackpkg/mirrors.bak | grep -m 1 "\# http://" | \
sed '/--/d'| sed 's/\#//g')

#You can test with nano +$LINE_MIROR /etc/slackpkg/mirrors
LINE_MIROR=$( cat /etc/slackpkg/mirrors | grep -n $MIRROR_URL | cut -d ":" -f 1 )
#MUL=$( sed -n -e ${LINE_MIROR},${LINE_MIROR}p /etc/slackpkg/mirrors.bak | sed 's/\#//g' > /etc/slackpkg/mirrors.old )

#MUL=$( sed -e "${LINE_MIROR}s's/\#//g'"/etc/slackpkg/mirrors.bak ) 


#Replace 
FILE_NEW=$( sed ${LINE_MIROR}s/\#//g /etc/slackpkg/mirrors.bak > /etc/slackpkg/mirrors.old )

if [ ! $? -ne 0 ]; then
	RUN  mv /etc/slackpkg/mirrors.old /etc/slackpkg/mirrors.new

	ORG_NO=$(cat /etc/slackpkg/mirrors | wc -l ) #Num lines
	EDIT_NO=$(cat /etc/slackpkg/mirrors.new | wc -l ) #Num lines														

	if [ "${ORG_NO}" -eq "${EDIT_NO}"  ]; then

		if mv /etc/slackpkg/mirrors.new /etc/slackpkg/mirrors ; then 	
			CMD "printf" "${GREEN}mirrorlist it's ok${NC}\n"; 

			RUN slackpkg update

			RUN slackpkg update gpg
		fi

	fi

fi


#sed -i '/^#.* slackware-13.0/ /s/^#//' /etc/slackpkg/mirrors/
#sed -i '' 's/^#commands/commands/g' /etc/slackpkg/mirrors/

# Replace mirrorlist with known fast and good Swedish mirror
#http://mirrors.slackware.com/mirrorlist/
#
#isos slackware 
#http://mirrors.slackware.com/slackware/slackware-iso/slackware-13.0-iso/


#slackpkg install-new

#slackpkg upgrade-all

#slackpkg clean-system

# Install base packages for me
RUN slackpkg install autoconf \
automake \
gcc \ 
kernel-headers \ 
libtool \
m4 \
make \
pkg-config

WORKDIR home && \
RUN pwd

#Repos https://slackbuilds.org/result/?search=package_name&sv=version
#http://packages.slackware.com/?search=firefox&release=slackware-13.0&mode=package&result=10&extra=on&patches=on&slackware=on
#http://packages.slackware.com/

#http://packages.slackware.com/?r=slackware-13.0&p=mozilla-firefox-3.6.28-i486-1_slack13.0.txz
#URL to download
#http://mirrors.slackware.com/slackware/slackware-13.0/patches/packages/mozilla-firefox-3.6.28-i486-1_slack13.0.txz

#pkgtool
#slackpkg install

#e.g. how to install easy mode - sublime text development
#google-chrome-stabale https://slackbuilds.org/result/?search=sublime&sv=14.1
#firefox https://slackbuilds.org/result/?search=sublime&sv=14.1

EXPOSE 1522
