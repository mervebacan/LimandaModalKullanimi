# **Liman MYS'de Modal Kullanımı**

###Modal Oluşumu Öncesi Gerekli Eylemler

Laravef tabanlı bir yazılım olup MVC mimarisi kullanılan Liman' a eklenti oluşturabilmek için giriş yapılır. İlk olarak **Sistem Ayarları > Eklenti > Yeni** yolu izlenerek eklenti oluşturulur. Eklenti sunucuya eklendikten sonra ssh ile Visual Studio Code' dan Liman sunucusuna bağlanılır.  

Oluşturulan eklenti dosyalarına Visual Studio Code ile eriştikten sonra gerçekleştirilecek ilk adım sekme oluşturmaktır. Bunun için onclick yapıldığında çalışması istenilen fonksiyon, **views > index.blade** içerisinde belirtilmelidir. Onclick ile file() fonksiyonun çalışılması istendiği gösterilmiş ve href ile de id' si tanımlanmıştır.

![2](https://user-images.githubusercontent.com/32592247/128641747-c7c12603-dd04-4ea3-8923-81d00ef52c6f.png)

'#' ile tanımlanılan id şu şekilde belirtilir.

![3](https://user-images.githubusercontent.com/32592247/128641813-82077bf0-4d69-46eb-923c-179952877832.png)

Eklentinin oluşturulan controllers kısmında command class' ının run fonksiyonu çalıştırılarak file' ı döndürmesi için argüman olarak file verilir. Çıktısı hazır respond fonksiyonu ile sağlanır. 

![13](https://user-images.githubusercontent.com/32592247/128641852-51610c4b-db5d-4929-8d20-451d38cbde03.png)

Belirtilen fonksiyon ve id' ye göre bir view oluşturulur.  View altında oluşturulan file klasörü içerisine main.blade ve script.blade dosyaları eklenir.

![4](https://user-images.githubusercontent.com/32592247/128641871-a69dd51c-24d9-4b9c-b8b6-480634844983.png)

Oluşturulan view, ana view' e entegre olarak dışardan çağırılabilir. Onclick e basıldığında çağırılacak fonksiyon, file içerisindeki script kısmında oluşturulur.

![5](https://user-images.githubusercontent.com/32592247/128641885-c1771775-9ef7-4d8b-af2b-b431b2cff9fb.png)

file() fonksiyonunun çalışabilmesi için, main kısmında row ve col-12 oluşturularak file.scripts, içe include edilir.

![6](https://user-images.githubusercontent.com/32592247/128641899-9b7d0dad-17a1-45e4-be3f-c0c385b48363.png)

Rotalar kısmında controllers'dan belirtilen get_files fonksiyonunu, yani FileController adıyla oluşturulan controllersın içerisindeki get fonksiyonunun çağırılması sağlanır.![rota](https://user-images.githubusercontent.com/32592247/128641917-6b9e8d80-8877-4e3f-b129-5491bbdc44a2.png)

### Modal Companent

Artık modal oluşturulması için ortam hazır. Modal oluşturulmadan önce, modalın açılabilmesi için bir modal-button oluşturulur. İkinci argüman olarak @include, genellikle array alır ve array, viewe gönderilen değişkenleri içerir. Düğmeye tıklandığında ne yazılacağı text kısmında ifadelendirilir. Class' ta buton tanımı yapılır, koddaki buton tanımı için "https://adminlte.io/themes/v3/iframe.html" sayfasından yararlanılmıştır. Açılacak modal ise target_id' de belirtilir. 

![7](https://user-images.githubusercontent.com/32592247/128641937-18c4b6db-5560-49b3-8559-e5690af605f7.png)

Örnek buton oluşturulmuş oldu. Ancak limanda butona tıklandığında, modalEvent fonksiyonu oluşturulmadığı için sayfa açılmaz. ![9](https://user-images.githubusercontent.com/32592247/128641953-fd1f59e5-b465-4653-a7ea-12dbb0a5cd4a.png)

Script kısmına geçilmeden önce modal compenenti tamamlayalım;

file klasörü içerisindeki main.blade dosyasında, @compenent ve @endcompanent ile companent yapısı açılır. İkinci argüman olarak array alan içerikte aynı şekilde ifadeler yazılır. Buradaki en önemli ifade ise id tanımlamasıdır.

![8](https://user-images.githubusercontent.com/32592247/128641987-4e17e4aa-6fb6-444d-94af-67af3aa87bcf.png)

Oluşturulan modal ismi id tanımlamasında belirtilirken, title değişkeni olarak gözükmesi istenilen isim verilir. Footer tanımlaması yine array şeklinde class, onclick ve text değişkenlerini içerir. Buradaki onclick, tıklandığında gereken işlemleri ifade eder ve createFileEvent fonksiyonu javascriptte yazılarak, açılmayan sekmenin açılmasını sağlar.

![scripts](https://user-images.githubusercontent.com/32592247/128642028-28d4d482-bf43-426a-912d-6ea4c5891e93.png)

FileController içerisindeki get fonksiyonun çağırılmasında; API' daki get_files fonksiyonuna gidilir ve buradan dönen response değeri, file class' ının içerisine yazdırılır. İşlem bitene kadar showSwal ile "Yükleniyor..." uyarısı çıkarılır.API' ya, yani controllersdaki get fonksiyonuna gidilerek veri gönderimi yapılması istenir. Ancak buradaki kodda veri kullanılmadığı için boş veri gönderilmiştir. Gidilen fonksiyon bir cevap alır ve dönen cevabın JSON olarak parçalanması istenir. İçerisindeki mesaj file class' ına, yani html içerisinde belirtilen kısma Jquery vasıtası ile text olarak yazdırılır. Ardından da swal uyarısı kapatılır.

Yapılan değişiklikler kaydedildikten sonra liman güncellenir ve modala tıklandığında grup başlığı, oluşturulan buton ve içerik ekranda gözlemlenir.

![15](https://user-images.githubusercontent.com/32592247/128642041-2194b7ab-7d02-4718-a27e-7ffc0d931727.png)

Sağ tarafta chrome consolu açılarak, oluştur butonuna tıklandığında tanımlanılan "İşlem Gerçekleşti" uyarısı gözlemlenmektedir.

### Modaldan Veri Alınması

Kullanıcıdan veri alınması için input'tan yararlanılır. Dosya ismi için isim girilmesi istenerek 'filename' şeklinde belirtilen id ile veri, main.blade kısmında alınır.

![16](https://user-images.githubusercontent.com/32592247/128642052-df4b5134-4491-43a4-b71b-147bfaf33859.png)

İnput içeriğinin alınması için javascriptte jquery ile modal id' si alınır. 'filename' id' li input'un içeriğinin alınması istenmektedir. Bu yüzden yazılan kodda da belirtildiği gibi, filename isimli id' ye sahip input'un value değeri alınır.

![17](https://user-images.githubusercontent.com/32592247/128642061-e1aec304-40ce-4ad3-ba09-5ce58c9e05d7.png)

Sonrasında da Swal.fire ile verinin alınıp alınmadığı gözlemlenir. Bu işlem console.log ile de gerçekleştirilebilir.

![18](https://user-images.githubusercontent.com/32592247/128642073-9061ca2c-bc32-4616-bb0b-823ba7b3397e.png)