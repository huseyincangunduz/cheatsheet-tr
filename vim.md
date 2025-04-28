# Vim Hile Kağıdı

```bash
vim file.txt # file.txt dosyasını text editör ile açar.
```

- Dosyadaki belli satıra ve harfe gitmek için Ok yön tuşları tuşları kullanılabilir. Dosyayı düzenlemek için "i" harfine basılarak İNSERT moduna geçilir. Düzenleme bittiği sırada "esc" ye basılarak İNSERT modundan çıkılır. 

- Belli görevler için **İNSERT modunda değilken** shift ve nokta'ya basılır.

    - Shift ve noktadan sonra kaydetmek için w yazılıp enterlanır

    - Shift ve noktadan sonra çıkmak için "q" yazılıp enterlanır (dosya kaydedilmedi ise hata verir)

    - Shift ve noktadan sonra çıkmak için "q!" yazılıp enterlanır (dosya kaydedilmediyse hata vermez, direkt çıkar)

    - Shift ve noktadan sonra kaydedip çıkmak için "wq" yazılıp enterlanır

    - Gitmek istediğiniz satır numarasını yazıp enterlarsanız o satır numarasına gider

    - "$" yazıp enterlarsanız dosyanın sonuna ulaşırsınız

    - "^" yazıp enterlarsanız dosyanın başına ulaşırsınız

- Vim'de ayrıca ok yön tuşları yerine "h"(sol), "j"(yukarı),"k"(aşağı),"l"(sağ) kullanılabilir. Ancak İNSERT modunda yön değiştirmek için Esc yapılmalıdır.


- Daha fazla detaylı bilgi için `vimtutor` komutunu çalıştırabilirsiniz