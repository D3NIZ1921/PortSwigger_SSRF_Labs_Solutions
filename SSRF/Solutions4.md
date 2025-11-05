Amaç, stok kontrol işlevi üzerinden localhost adresine istek göndererek yetkilendirme kontrolünü atlamaktır. <br>

Aşama 1: Hedefin Belirlenmesi
Açıklama: ![Aciklama](1.png) <br>
Lab'ın temel SSRF saldırısı hakkında genel bilgi ve labın durumunu (Not solved) gösterir. Saldırı, bir URL belirtilerek stok durumu isteyen bir işlevi sömürmeyi amaçlar. <br>

Hedef: ![Hedef](4.png) <br>
Stok kontrol URL'sini http://localhost/admin olarak değiştirmek ve carlos kullanıcısını silmektir. <br>

Arayüz: ![Arayuz](2.png) <br>
Ürün sayfasında (Picture Box) bulunan ve SSRF zafiyetine yol açan "Check stock" düğmesi görülmektedir. <br>

Admin Paneli: ![Panel](6.png) <br>
Sayfanın alt kısmında, yetkili olunmadığı için erişilemeyen veya görünmeyen Yönetici Paneli'nin bir kısmı ve silinmesi gereken carlos kullanıcısı listelenir. <br>

Aşama 2: İstek Yakalama ve Repeater'a Gönderme <br>
Burp Intercept: ![Burp](3.png) <br> Burp Suite ile stok kontrolünü tetikleyen POST isteği yakalanır. İstek, işleme kolaylığı ve tekrar denemeler için "Send to Repeater" (Yeşil ok) seçeneği ile Repeater sekmesine yönlendirilir. <br>

Aşama 3: Dahili Adrese Erişim Denemesi <br>
Burp Repeater - Panel: ![Repeater](5.png) <br>
İstek Repeater sekmesine taşınmıştır. SSRF zafiyetinin bulunduğu stockApi: parametresi değiştirilerek yönetici arayüzüne erişim denenir. <br>

Payload: stockApi:http://localhost/admin <br>

Amaç: Bu istek, sunucunun dahili olarak localhost adresine bağlanmasını sağlayarak, genellikle sadece yerel döngü (loopback) üzerinden gelen isteklere güvenen güvenlik mekanizmasını atlamayı hedefler. <br>

Admin Paneli: ![Panel](7.png) <br>
http://localhost/admin adresine erişim sağlandıktan sonra, eğer admin arayüzü sadece yönetici olarak oturum açılmışsa veya loopback'ten istenmişse erişilebilirse, bu fotoğraf dönebilir ve admin arayüzünün varlığını doğrular. <br>

Aşama 4: Nihai Sömürü ve Kullanıcı Silme <br>
Silme Denemesi: ![Delete](8.png) <br>
Bu fotoğraf, tarayıcıda doğrudan admin silme komutunun denenmesini gösterir (/admin/delete?username=carlos). Bu dışarıdan erişilemez ve lab hedefine ulaşmak için SSRF kullanılmalıdır. <br>

Silme Payload'u: ![Silme](9.png) <br>
Çözüm için Burp Intercept sekmesinde (veya Repeater'da) nihai payload hazırlanmıştır. <br>

Nihai Payload: stockApi:http://localhost/admin/delete?username=carlos (Yeşil ok ile gösterilen kısım). <br>

Bu istek sunucuya gönderildiğinde, sunucu dahili olarak localhost adresindeki admin paneline erişir ve carlos kullanıcısını silme işlemini gerçekleştirir. <br>

Aşama 5: Bitiş <br>
Finish: ![Finish](10.png) 
