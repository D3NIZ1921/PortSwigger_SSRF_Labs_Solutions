1. Lab Hedefinin Anlaşılması: ![Start](1.png) <br>
Hedef: Labın temel amacı, stok kontrol özelliğindeki SSRF zafiyetini kullanarak, karaliste tabanlı filtrelemeyi atlatmak ve dahili yönetici arayüzü olan http://localhost/admin adresine erişmektir. <br>
Son İşlem: Yönetici arayüzüne ulaşıldıktan sonra, carlos kullanıcısının silinmesini sağlayan komutu tetiklemektir. <br>
Arayüz: ![check_stock](2.png) <br>
Lab'a erişim sağlandığında, "WTF? - The adult party game" ürününün detay sayfası görülür. Sayfanın alt kısmında, zafiyetin bulunduğu potansiyel nokta olan "Check stock" (Stok Kontrol) düğmesi yer almaktadır. <br>
Aşama 2: İstek Yakalama ve Payload Hazırlığı <br>
Burp Suite: ![Burp](3.png) <br>
Ürün sayfasında "Check stock" işlemi tetiklendiğinde, Burp Suite'in Proxy -> Intercept sekmesinde bir POST isteği yakalanır. Bu istekte, sunucunun dahili bir sistemden stok bilgisini çektiği StockApi: parametresi görülmektedir. <br>
Bu aşamada, zafiyetin sömürülmesi için hedef URL http://127.0.0.1/admin/delete?username=carlos olarak ayarlanmıştır. <br>
Filtre Atlama: ![Burp_decoder](4.png) <br>
Black-list filtrelemesini atlatmak için URL Kodlama (Encoding) teknikleri denenmiştir. Fotoğrafta, Burp Decoder sekmesi kullanılarak / gibi kritik karakterlerin çift URL kodlama ile (%2f $\to$ %252f) filtreyi nasıl atlayacağı araştırılmıştır. <br>
Bu, filtrenin sadece tekil kodlanmış tehlikeli karakterleri algıladığı varsayımına dayanır. <br>
Aşama 3: Saldırı Payload'unun Gönderilmesi <br>
Payload: ![Burp](5.png) <br>
Bu fotoğraf, saldırı payload'unun son halini gösterir. Burp Suite'in Proxy ekranında yakalanan POST isteğinin gövdesindeki StockApi: parametresi, lab hedefini gerçekleştiren URL ile değiştirilir: http://127.0.0.1/admin/delete?username=carlos. <br>
Bu URL, filtrenin atlatıldığı bir formattadır. <br>
Forward: ![Forward](6.png) <br>
Payload'umuzu forward ediyoruz. <br>
Tamamlama: ![Finish](7.png) <br>
Değiştirilmiş payload sunucuya iletildikten sonra, sunucu dahili olarak filtreyi atlayarak yönetici arayüzüne istek göndermiştir. Bu istek, carlos kullanıcısını silme işlemini başarıyla gerçekleştirmiştir.
