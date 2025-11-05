Bu lab, Burp Suite kullanılarak bir web uygulamasındaki açık yönlendirme (Open Redirection) zafiyetini bir SSRF filtresini atlatmak için kullanılmasından ibarettir. <br>
Aşama 1: Hedef ve Kullanılan Teknik <br>
Lab Tanımı: ![Tanım](1.png) <br>
Bu aşama, çözümün amacını belirtir: Stok kontrol özelliğindeki SSRF zafiyetini kullanarak, dahili adresteki http://192.168.0.12:8080/admin arayüzüne erişmek ve carlos kullanıcısını silmek. Filtre, yalnızca yerel erişime izin verdiği için, bir açık yönlendirme zafiyetinin kullanılması gerekmektedir. <br>

Aşama 2: Uygulama Arayüzü ve Admin Paneli <br>
Arayüz: ![Arayüz](2.png) <br> 
"First Impression Costumes" ürün sayfasının altındaki "Check stock" (yeşil ok) düğmesi, sunucu tarafında istek (SSRF) gönderen işlevi tetikleyen noktadır. <br>

Admin Paneli: ![Panel](4.png) <br> 
Lab başlangıcında, yönetici panelinin varlığı, carlos kullanıcısının listelendiği ve labın henüz çözülmediği (LAB Not solved) görülmektedir. Nihai hedef, bu kullanıcıyı silmektir. <br>

Aşama 3: Saldırı İsteğinin Düzenlenmesi <br>
Admin Panel Keşfi: ![Keşif](3.png) <br> 
Burp Suite'te yakalanan POST isteği görülür. StockApi: parametresi, zafiyetin sömürüldüğü alandır. Filtreyi atlamak için, uygulamanın kendi içerisindeki bir açık yönlendirme zafiyeti kullanılır. <br>

Payload (Keşif Aşaması): stockApi:/product/nextProduct?productId=1&path=http://192.168.0.12:8080/admin (Yeşil ok, payload'un başlangıçta admin panelini hedef aldığını gösterir). Bu payload, sunucuyu önce kendi üzerindeki bir yönlendirme endpoint'ine, oradan da kısıtlı dahili adrese yönlendirir. <br>

Denemeler: ![Payload](5.png) <br> 
Tarayıcıda doğrudan denenen bir URL'yi gösterir. Buradaki http://192.168.0.12:8080/admin/delete?username=carlos adresi dışarıdan erişilemediği için tarayıcıda "Not Found" (veya benzeri bir hata) dönebilir. Bu fotoğraf, dahili adresin harici erişime kapalı olduğunu teyit eder. <br>

Aşama 4: Nihai Sömürü ve Kullanıcı Silme <br>
Payload: ![Payload2](6.png) <br> Keşif sonrası, carlos kullanıcısını silmek için payload güncellenir. <br>

Nihai Payload: stockApi:/product/nextProduct?productId=1&path=http://192.168.0.12:8080/admin/delete?username=carlos (Yeşil ok). <br>

Bu istek, Burp'te "Forward" edilerek sunucuya gönderilir. Sunucu, açık yönlendirme zafiyetini takip ederek dahili IP adresine ulaşır ve carlos kullanıcısını siler. <br>

Aşama 5: Labın Başarıyla Çözülmesi <br>
Finish: ![Finish](7.png) <br>
Nihai isteğin gönderilmesinin ardından, lab hedefine ulaşıldığı görülür.
