
# 🖧 ft\_irc - Internet Relay Chat Sunucusu

## 📌 Proje Hakkında

**ft\_irc**, **C++98** ile geliştirilmiş basit bir **IRC (Internet Relay Chat) sunucusudur**.\
Bir **IRC istemcisi** kullanılarak bağlanabilir ve TCP/IP üzerinden çoklu istemci desteğiyle gerçek zamanlı mesajlaşma sağlar.

---

## 🚀 Özellikler

- **poll()** (veya eşdeğeri) ile **çoklu istemci desteği**
- **Kimlik doğrulama sistemi** (`PASS`, `NICK`, `USER` komutları)
- **Gerçek zamanlı mesajlaşma** (kanallar ve özel mesajlar)
- **Kullanıcı rolleri** (Operatörler ve normal kullanıcılar)
- **Uygulanan IRC komutları:**
  - `JOIN` - Bir kanala katıl
  - `PART` - Bir kanaldan ayrıl
  - `PRIVMSG` - Özel mesaj gönder
  - `KICK` - Bir kullanıcıyı kanaldan at
  - `INVITE` - Bir kullanıcıyı kanala davet et
  - `TOPIC` - Kanalın konusunu değiştir veya görüntüle
  - `MODE` - Kanal modlarını yönet (`+i`, `+t`, `+k`, `+o`, `+l`)
  - `QUIT` - Sunucudan çıkış yap
- **Mesaj yayını** - Kanaldaki tüm kullanıcılar mesajları alır.
- **Hata yönetimi** - Geçersiz komutlar ve yanlış girişler için uyarılar sağlar.
- **Temiz ve modüler kod** **NYP (Nesne Yönelimli Programlama)** prensiplerine uygun olarak yazılmıştır.

---

## ⚙️ Kurulum ve Derleme

### Gereksinimler

- **C++98 uyumlu bir derleyici**
- **Bir IRC istemcisi** (`irssi`, `WeeChat`, `HexChat` veya test için `netcat` gibi)

### Derleme

Projeyi derlemek için aşağıdaki komutu çalıştırın:

```sh
make
```

Bu işlem tamamlandığında **ircserv** adlı çalıştırılabilir dosya oluşturulacaktır.

---

## 🔧 Kullanım

Sunucuyu başlatmak için:

```sh
./ircserv <port> <password>
```

- `<port>`: IRC sunucusunun dinleyeceği bağlantı noktası.
- `<password>`: Bağlanmak için istemcilerin kullanacağı şifre.

---

## 🔗 IRC Sunucusuna Bağlanma

### 🔹 Terminal Üzerinden Bağlantı (`HexChat` ile Test)

Bağlantıyı test etmek için aşağıdaki komutu çalıştırın:

```sh
nc -C 127.0.0.1 6667
```

Daha sonra kimlik doğrulama yaparak giriş yapabilirsiniz:

```irc
PASS mypassword
NICK mynickname
USER myusername 0 * :My Real Name
```

Bir kanala katılmak için:

```irc
JOIN #mychannel
```

---

## 📜 Desteklenen IRC Komutları ve Örnekler

| Komut     | Açıklama                           | Örnek                           |
| --------- | ---------------------------------- | ------------------------------- |
| `PASS`    | Şifre ile kimlik doğrulama         | `PASS mypassword`               |
| `NICK`    | Kullanıcı adını belirle            | `NICK Alice`                    |
| `USER`    | Kullanıcı adı ve gerçek adı ayarla | `USER Alice 0 * :Alice Doe`     |
| `JOIN`    | Bir kanala katıl                   | `JOIN #channelname`             |
| `PART`    | Bir kanaldan ayrıl                 | `PART #channelname`             |
| `PRIVMSG` | Özel mesaj gönder                  | `PRIVMSG Bob :Merhaba!`         |
| `KICK`    | Kullanıcıyı kanaldan at            | `KICK #channelname Bob :Sebep`  |
| `INVITE`  | Bir kullanıcıyı kanala davet et    | `INVITE Bob #channelname`       |
| `TOPIC`   | Kanal konusunu değiştir/görüntüle  | `TOPIC #channelname :Yeni konu` |
| `MODE`    | Kanal ayarlarını değiştir          | `MODE #channelname +i`          |
| `QUIT`    | Sunucudan çıkış yap                | `QUIT`                          |

---

## 🖧 **Socket Programlama & Sunucu Mantığı**

Bu IRC sunucusu istemcilerle iletişim kurmak için **soket programlamayı** kullanır. **Socket**, ağ üzerinden veri alışverişi yapmak için kullanılan bir uç noktadır.

### 🏗️ **Socket İşleyişi**

1. **Socket oluşturma** - `socket()` fonksiyonu ile başlatılır.
2. **Bağlantı noktası ile eşleme (bind)** - `bind()` fonksiyonu ile belirli bir **port** ile eşlenir.
3. **Bağlantıları dinleme (listen)** - `listen()` ile bağlantıları dinlemeye başlar.
4. **İstemcileri kabul etme (accept)** - `accept()` çağrıldığında istemci bağlantısı kabul edilir.
5. **Veri gönderme ve alma (send/recv)** - İstemcilerle mesajlaşma `send()` ve `recv()` fonksiyonları ile sağlanır.
6. **Bağlantıyı kapatma** - `close()` fonksiyonu ile istemci bağlantısı kapatılır.

### **Sunucunun Ana Döngüsü**

Sunucu, bağlı istemcileri sürekli **poll()** fonksiyonu ile kontrol eder ve gelen mesajları işler:

```cpp
int retval = poll(&_pollFds[0], _pollFds.size(), -1);
if (retval == -1) {
    std::cerr << "poll Error: " << strerror(errno) << std::endl;
}
```

Bu sayede, **non-blocking (engelleyici olmayan) soketler** kullanarak etkin bağlantı yönetimi sağlar.

---

## 📂 Proje Yapısı

```
├── src/
│   ├── Server.cpp       # Sunucu mantığı
│   ├── Client.cpp       # İstemci yönetimi
│   ├── Channel.cpp      # Kanal işlemleri
│   ├── Cmds.cpp         # Temel komutlar
│   ├── Cmds1.cpp        # Ek komutlar
│   ├── Cmds2.cpp        # Kanal modları
│   ├── Tools.cpp        # Yardımcı fonksiyonlar
│   ├── main.cpp         # Giriş noktası
├── includes/
│   ├── ft_irc.hpp       # Ana başlık dosyası
│   ├── rpls_and_errs.hpp # Hata ve yanıt mesajları
├── Makefile             # Derleme betiği
├── README.md            # Dokümantasyon
```

---

## 🎉 Özel Teşekkür

Bu proje takım arkadaşlarım **ydunay** ve **zeatilga** ile birlikte geliştirilmiştir.\
Destekleri için ekibime **teşekkür ederim!** 🙌

---

🚀 **İyi kodlamalar!** 💻🎯

