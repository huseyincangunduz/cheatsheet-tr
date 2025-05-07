# Bash Scriptleri

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

## Parametreler

Bash scriptiniz, parametreleri `$1, $2, $3 ... $n` şeklinde almaktadır

```bash
#script.sh
echo "Merhaba $1 $2. İsminin: $1 ve soyisminin $2 olduğunu biliyorum"
```

```bash
./script.sh Hüseyin Gündüz
# Merhaba Hüseyin Gündüz. İsminin: Hüseyin ve soyisminin Gündüz olduğunu biliyorum
```

Eğer arasında boşluk varsa, bir parametre değerini `"` ile sarabilirsiniz.

```bash
./script.sh Hüseyin Can Gündüz
#Çıktı: Merhaba Hüseyin Can. İsminin: Hüseyin ve soyisminin Can olduğunu biliyorum

./script.sh "Hüseyin Can" Gündüz
#Çıktı: Merhaba Hüseyin Can Gündüz. İsminin: Hüseyin Can ve soyisminin Gündüz olduğunu biliyorum

# NOT: Ayrıca kaçış parametresi ile (\ ) kullanarak da tek parametre olarak algılamasını sağlayabilirsiniz

./script.sh Hüseyin\ Can Gündüz
#Çıktı: Merhaba Hüseyin Can Gündüz. İsminin: Hüseyin Can ve soyisminin Gündüz olduğunu biliyorum

```

## Değişkenler

Script içinde değişken kullanabilirsiniz.

```bash
### script.sh
DESTINATION=Yenikapı
echo "Aracımız $DESTINATION yönüne gidecektir"

### bash
./script.sh
# Çıktı: Aracımız Yenikapı yönüne gidecektir
```

Eğer script içinde tanımlanmadıysa, ayrıca dışarıdan `export` keywordü ile tanımlanabilir.

```bash
### script.sh
echo "Aracımız $DESTINATION yönüne gidecektir"

### bash
DESTINATION="Atatürk Havalimanı"
./script.sh
# Çıktı: Aracımız  yönüne gidecektir
export DESTINATION="Atatürk Havalimanı"
./script.sh
# Çıktı: Aracımız Atatürk Havalimanı yönüne gidecektir
```

> İf, for kondisyonları ile ilgili daha fazla detayı `bash-script-extra.md` ye göz atabilirsiniz.

## If koşulu

### String kontrolü

```bash
### script.sh

if [ "$HE_IS" = "doktor" ]; then
    echo "Aaa sen Zenan değil misin Leventin sevgilisi?..."
else
    echo "Siz doktor DEĞİLSİNİZ! Şırınganın içinde hava boşluğu var."
fi

### Terminal

HE_IS="doktor" ./script.sh
# Çıtkı: Aaa sen Zenan değil misin Leventin sevgilisi?...
HE_IS="mühendis" ./script.sh
# Çıtkı: Siz doktor DEĞİLSİNİZ! Şırınganın içinde hava boşluğu var.
HE_IS="Doktor" ./script.sh # DİKKAT: BÜYÜK/KÜÇÜK HARF DUYARLIDIR
# Çıtkı: Siz doktor DEĞİLSİNİZ! Şırınganın içinde hava boşluğu var.
```

### Sayı kontrolü

```bash
### script.sh
if [ 5 -gt 10 ]
then
   echo "number is greater than 10"
else
   echo "number is less than 10"
fi
# Çıtkı: number is less than 10
```

`-gt` yerine kullanılabilecek diğer operatörler

- `-gt`: Greater than (Büyüktür)
- `-gte`: Greater than or equal (Büyüktür ya da eşit)
- `-lt`: Less than (Küçüktür)
- `-lte`: Less than or equal (Küçüktür ya da eşit)
- `=` ya da `-eq`: Equals (Eşittir)

### Çoklu durumlar

- `||` ya da `-o`: or (ya da)
- `&&` ya da `-a` : and (ve)

```bash
### OR KULLANIMI
OS="windows"
if [ "$OS" = "linux" ] || [ "$OS" = "macos" ]; then
    echo "Linux ya da Macos"
else
    echo "Linux ya da Macos değil"
fi


### OR KULLANIMI 2
#Çıktı: Linux ya da Macos değil
OS="linux"
if [ "$OS" = "linux" ] || [ "$OS" = "macos" ]; then
    echo "Linux ya da Macos"
else
    echo "Linux ya da Macos değil"
fi
#Çıktı: Linux ya da Macos


### İÇ İÇE VE AND
OS="windows"
DISTRO="11"
if [ "$OS" = "macos" ]; then
    echo "Macos kullanıyorsun"
elif [ "$OS" = "linux" ] && [ "$DISTRO"="arch" ]; then
    echo "Arch kullanıyorsun"
elif [ "$OS" = "linux" ]; then
    echo "Farklı linux dağıtımı kullanıyorsun"
else
    echo "farklı bir işletim sistemi kullanıyorsun"
fi
#Çıktı: farklı bir işletim sistemi kullanıyorsun

### İÇ İÇE VE AND 2
OS="linux"
DISTRO="fedora"
if [ "$OS" = "macos" ]; then
    echo "Macos kullanıyorsun"
elif [ "$OS" = "linux" ] && [ "$DISTRO" = "arch" ]; then
    echo "Arch kullanıyorsun"
elif [ "$OS" = "linux" ]; then
    echo "Farklı linux dağıtımı kullanıyorsun"
else
    echo "farklı bir işletim sistemi kullanıyorsun"
fi
#Çıktı: Farklı linux dağıtımı kullanıyorsun

### İÇ İÇE VE AND 2 ama bir terslik var
OS="linux"
DISTRO="fedora"
if [ "$OS" = "macos" ]; then
    echo "Macos kullanıyorsun"
### eşittir ("=") aralarnda mutlaka BOŞLUK KULLANIN
elif [ "$OS" = "linux" ] && [ "$DISTRO"="arch" ]; then
    echo "Arch kullanıyorsun"
elif [ "$OS" = "linux" ]; then
    echo "Farklı linux dağıtımı kullanıyorsun"
else
    echo "farklı bir işletim sistemi kullanıyorsun"
fi
#Çıktı: Arch kullanıyorsun
```

> For döngüsü için daha sonra yazıyor olacağım... (umarım)
