
                          📖 ## USEFUL TIPS ## DPD SQUADS ## 📖


# 🔎 DELL NETWORKER



#  📑 Documentação 

 - [Docs Networker](https://www.dell.com/support/home/en-us/product-support/product/networker/docs)

 - [Command Reference Guide](https://www.delltechnologies.com/asset/en-us/products/data-protection/technical-support/docu96459.pdf)

 - [DR Networker](https://www.dell.com/support/manuals/en-us/networker/nw_p_server_disasterrecovery/using-nsrdr-to-perform-a-disaster-recovery?guid=guid-73d0446f-831c-453c-929f-6025456e56ec&lang=en-us)


============================================================================================================



### 📑TIPS ###



#### ➡️ Gerando o NSRGET ###
 

Descarregue do FTP abaixo o utilitário NSRGET para o sistema operacional de cada um dos equipamentos abaixo:
servidor Networker
Storage Nodes
https://central.dell.com/solutions/Networker-Tools
nsrget.zip  (Windows SO)
nsrget.tar  (Linux/Unix SO)

Após descompactar o arquivo baixado, execute o comando abaixo para iniciar a coleta
(Windows)C:\>nsrget.bat -o:E –p:c:\<DiretorioVazio> <Enter>
(Linux/Unix) #nsrget.sh-o:E –p:/< DiretorioVazio > <Enter>

Ao final da execução, o utilitario criara um arquivo ZIP na direção do parâmetro “-p:” do comando.

Após descompactar o arquivo baixado, execute o comando abaixo para iniciar a coleta
(Windows)C:\>nsrget.bat -o:E –p:c:\<DiretorioVazio> <Enter>
(Linux/Unix) #nsrget.sh-o:E –p:/< DiretorioVazio > <Enter>

Ao final da execução, o utilitario criara um arquivo ZIP na direção do parâmetro “-p:” do comando.




### ➡️ Limpando cache de Java em embientes linux ###


javaws -clearcache



### ➡️ Verificando o processo da workflow ###

ps -ef | egrep -i "workflow name"



### ➡️ Checando versao do NW / Client / SN ###

rpm -qa | egrep -i "lgto"



### ➡️ Network Conections check ###

 nsradmin -s (SN) -p nsrexec from NW server to SN
 nsrrpcinfo -P
 nsrnmmsv -P  (Validar comunicacao do NMM com windows)



### ➡️ Verificar validação de certificados ###

echo del type: nsr peer information > delpeer.nas nsradmin -i delpeer.nas -p nsrexec



### ➡️ Parar serviços Networker ###


nsr_shutdown

net stop nsrexecd   

### ➡️ Listar serviços ###

tasklist |findstr nsr (Windows)
ps -ef | grep nsr (LINUX)

### ➡️ Iniciar os serviços do Networker ###

net start nsrd & net start gstd



### ➡️ Limpando as chaves de autenticação  (sempre fazer no NW server / sotrage node / Agents) ###

echo del type: nsr peer information > delpeer.nas

nsradmin -i delpeer.nas -p nsrexec



### ➡️ versao do pacote NETWORKER ###

linux
rpm -qa | grep lgto

windows 
tasklist | finstr nsr



### ➡️ reset Tape library ###

nsrjv -HEv no storage node

inquire -lscp

changers -v

nsrjb -IIE (Inventário rápido)



### ➡️ Renderizando logs ###

nsr_render_log -typemac daemon.raw > daemon.raw.txt

Por periodo

nsr_render_log -L en_DK -S "08/06/2023 06:00" -E  "08/06/2023 08:00" /nsr/logs/daemon.raw > /08-06-0600-0800.log 2>&1



### ➡️ VCENTER ###

curl -k -i -u administrator@vsphere.local  "url host "


# Desregistrando o  Vproxy  ##

https://www.dell.com/support/kbdoc/en-us/000197856

cd /opt/emc/vproxy/runtime/state   
 
mv vproxyState.dat vproxyState.dat_old

echo UNREGISTERED > vproxyState.dat

cat VsproxyState.dat

systemctl restart vbackupd

reboot



### ➡️ restore media Database ###

nsrmmdbasm -2 -r < mm.xdr

Export
 
nsrmmdbasm -s /nsr/mm/mmvolrel > mm.xdr



### ➡️ MMINFO ###

mminfo -B  para ver o bootstrap

mminfo -q ""SSID=" -s

mminfo -q "client=" -s

mminfo -av -r volume  Lista volumes

mminfo -mvV -q "type=Data domain" 

mminfo -mvV -q "type=DD Cloud Tier"

mminfo -avot -q suspect -r ssid,cloneid,volume

mminfo -S -q ssid="SSID"

minfo -avot -q "ssattr=*BlockBasedBac

mminfo -avot -q "ssattr=*BlockBased Virtual Full"

mminfo -avot -q "ssattr=*Synthetic full"

mminfo -avot -q  sscreate<=<<=30 days ago (Maiores que 30 dias)

mminfo -avot -xc, -r ssid,cloneid,totalsize,client,name,sscreate,clretent,volume,pool,sumflags,level,copies -q type='Data Domain' -t '2 month ago' >> Path_to_save

mminfo -avot -q "client=orajfp02-vip-bkp.mrs.com.br" -r "sscreate,client,ssflags,name,group,ssid,ssretent,level,volume,type,copies,clretent,totalsize(7)" -xc"

mminfo -avot -q "ssretent<=yesterday,type=Data Domain" -r "ssid,cloneid,ssretent,client,volume,ssflags,sumflags,totalsize(7),name" > suspected_ssids.txt

mminfo -otc -xc, -q 'name=Q154BSSPED01.br154.corpintra.net' -r "name,volume,copies,type,ssid,cloneid,clretent"

mminfo -otc -xc, -av -r "name,volume,copies,type,ssid,cloneid,clretent" -q "ssid=2297190828"

mminfo -avot -k -t 07/08/2021 -r "vmname,sscreate,ssretent,volume,ssid" |egrep -i "APSPOS35W1P"

mminfo -avot -xc, -q "copies>2" -r "ssid,cloneid,sscreate,ssretent,volume,name" -t '1 month ago'

mminfo -av -q "type=Data Domain, type=DD Cloud Tier" -r "ssid(54),location,type,ssretent,ssflags,cloneid,clretent,totalsize,sumsize,client,volume,clflags,copies" "-xc|" > dados.txt

mminfo -avot -xc, -r "ssid,name,group,client,volume,ssflags,clflags,sumflags,action" -q "client=client_name" -t "MM/DD/AAAA"




###  ➡️ Deletar SSIDS ###

for i in $( cat /home/bferraz.cl/ssid.txt ) ; do nsrmm -d -S ${i} -y ;done



### ➡️ Retirar SSIDs de SUSPECT ###

for i in $( cat /home/bferraz.cl/ssid.txt ) ; do nsrmm -S ${i} -o notrecyclable -y ;done

for i in $( cat /home/bferraz/stage/ssid.txt ) ; do nsrmm -S $i -o notsuspect -y ;done



### ➡️ Apagar dados expirados nos volumes ###


nsrstage -C -V " Volume Name"  (procura dados orfãs no DD)

nsrim -X  (Efetua a limpeza no NW)



### ➡️ Reset permissionamentos  authc ###

/opt/nsr/authc-server/scripts/authc_configure.sh



### ➡️ VSS ###
vssadmin create shadow /for=c:   (força a criaçào do shadow no c:\)



### ➡️ Fazendo backup manual ###

Save -D5 -s cbrcur01bkxp190bkp.cnh1.cnhgroup.cnh.com -c cbrcur01sqcc040bkp.cnh1.cnhgroup.cnh.com -B pool_fs_homolog c:\temp
D5  debug
-s NW server
-C client
-B Pool



### ➡️ Process Filters ###

fltmc filter

fltmc volumes 

fltmc intance




### ➡️ ORACLE  (NMDA) ###

Client oracle

cd $ORACLE_HOME/lib 
ls -las libobk*

libobk.so -> /usr/lib/libnsrora.so
ln -s /us	r/lib/libnsrora.so libobk.so
ls -las libobk*
cd /usr/lib
ls |egrep -i libnsr





### ➡️ Problemas com o Servidor de console NW   (NMC) ###

 systemctl stop gst (console JAVA)
 nsr/nmc/ mv nmcdb nmcdb_old
 systemctl start gst

Se o NMC nao abrir

cd /opt/lgtonmc/bin
Executar o script ./nmc_config
Verificar o serviço systemctl status gst


NWUI (Console HTML5)

cd /opt/nwui/scripts/



### ➡️ VMWARE ###



- Verificando status dos serviços no Vcenter:

service-control --status

- Logs do Vcenter

vpxd.log   (https://kb.vmware.com/s/article/1021804)




### ➡️ Recovery ###

recovery.exe -s "NW server" -iN -nv -d C:\restore -S "SAVESET ID" > c:\restore\recover-S-savesetID.txt




### ➡️ Restore CLIENT INDEX ###

mminfo -avot -xc, -r "ssid,name,group,client,volume,ssflags,clflags,sumflags,action" -q "client=client_name" -t "MM/DD/AAAA"

recover -s <nwserverhost> -S <ssid-of-the-index-saveset>

nsrck -t "mm/dd/yyyy" -L7 client_name



