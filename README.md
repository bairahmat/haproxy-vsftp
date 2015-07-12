# VSFTP Configuration: 
###(vsftpd: version 3.0.2)

	listen=YES
	#notice that ftp is NOT running on the default port 21
	listen_port=211
	connect_from_port_20=NO
	write_enable=YES
	...
	tcp_wrappers=YES
	pasv_address=45.55.164.239
	pasv_enable=YES
	pasv_promiscuous=YES
	port_enable=YES
	port_promiscuous=YES
	pasv_min_port=10000
	pasv_max_port=10250
	seccomp_sandbox=NO

# HAProxy Configuration 
###(HA-Proxy version 1.4.24)

	#HAProxy listens on port 21, and proxies to port 211 where the ftp server is running
	listen FTP :21, :10000-10250
		mode tcp
		server ftp01 45.55.164.239:211 check port 21


#Demo
###(Option "-A" forces ftp into active mode. Passive works fine too)

	jsnavely@Jims-MacBookPro ~/Dropbox/haproxy_scott $ ftp -A ftptest@45.55.164.239
	Connected to 45.55.164.239.
	220 (vsFTPd 3.0.2)
	331 Please specify the password.
	Password: 
	230 Login successful.
	Remote system type is UNIX.
	Using binary mode to transfer files.
	ftp> put settings.md 
	local: settings.md remote: settings.md
	200 EPRT command successful. Consider using EPSV.
	150 Ok to send data.
	100% |***********************************************************************|   586        6.07 MiB/s    00:00 ETA
	226 Transfer complete.
	586 bytes sent in 00:00 (9.12 KiB/s)
	ftp> 


