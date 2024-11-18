# IPTV-CCTV-Dockerized



## Credit

- https://github.com/youshandefeiyang/allinone

- https://github.com/containrrr/watchtower



## Best IPTV Player

- WIndows: PotPlayer - All-functional, Live-stream loading very fastly
- macOS: IINA - Beautiful GUI, smooth experience
- Linux: VLC Media Player - light-weight, high adaptivity
- Android (phone or TV Box): mytv, TiviMate
- iOS: ntPlayer, APTV - multi-format, easy to use



## Environment

- Docker system has been installed



## Install - Method 1

1. Deployment - SSH to NAS or computer, then run the command to run the docker of allinone:

   ```
   docker run -d --restart unless-stopped --net=host --privileged=true -p 35455:35455 --name allinone youshandefeiyang/allinone
   
   ## COnfigure watchtower listening to the update commander in allinone image
   docker run -d --name watchtower --restart unless-stopped -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower allinone -c --schedule "0 0 2 * * *"
   ```
   
2. TV Channel live streaming (youshandefeiyang/allinone) - Regrouping and Optimizing

   ```
   docker run -d --restart=always -p 35456:35456 --name allinone_format yuexuangu/allinone_format:latest
   ```

3. Request re-grouped m3u file from the IPTV Player:

​	Format: `http://<Server-IP>:35456/tv.php?h=<Server-IP>&p=<allinone-Port>&m=1&t=0`

​	Parameters:

​	h - Optional,  can be public Ip address or local IP address (default), 127.0.0.1 is not allowed

​	p - Optional, deployment port, default = 35455 (same as Step 1)

​	m - Optional, default=1, re-group the TV channels (recommended); 0- meaning not re-group

​	t - Optional, output format, fedault=0 (m3u), aother value=1 (text)

​	Example:

​		http://172.20.10.8:35456/tv.php

​	This means:

​		http://172.20.10.8:35456/tv.php?h=172.20.10.8&p=35455&m=1&t=0



## Install - Method 2 (No re-grouping)

- Deploy - Use docker-compose.yml

  ```
  cd arm
  nano docker-compose.yml
  ## The av3a-assitant was on port 35352, actually it's 4K 8K video, need a confirmation
  ## The regular channels are at port 35455
  
  docker-compose up -d
  ```

- Enjoy the IPTV with any IPTV Player.

  ```
  ## Then we can use IPTV Player to open URL:- Regular channels
  http://<your-IP-Address>:35455/tv.m3u
  
  ## 4k 8K channels - need a stronger GPU
  http://<Your-IP-Address>:35452/tv.m3u
  ```



## License

MIT
