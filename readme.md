# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
     SELECT ogrenci.ograd,ogrenci.ogrsoyad,islem.atarih FROM islem , ogrenci  WHERE  ogrenci.ogrno=islem.ogrno ORDER BY ogrenci.ograd;
	
	 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	

     SELECT kitapadi,turadi FROM tur , kitap  where tur.turno=4=kitap.turno or  tur.turno=6=kitap.turno;

	
	  3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	

     SELECT ogrenci.ogrno,ograd,ogrsoyad,kitapadi FROM ogrenci,islem,kitap where (sinif='10A' OR'10C' ) AND ogrenci.ogrno=islem.ogrno and kitap.kitapno=islem.kitapno;


	#join ile yazın
	 4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	

    SELECT ograd,ogrsoyad,atarih FROM ogrenci INNER JOIN islem ON ogrenci.ogrno=islem.ogrno;  

     5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	

    SELECT kitapadi,turadi FROM tur INNER JOIN kitap ON  tur.turno=4=kitap.turno OR tur.turno=6=kitap.turno;

	
	 6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	

       SELECT ogrno,ograd,ogrsoyad,kitapadi,sinif FROM ogrenci 
	   INNER JOIN islem ON ogrenci.ogrno=islem.ogrno 
	   INNER JOIN kitap ON kitap.kitapno=islem.kitapno 
	   WHERE (ogrenci.sinif='10c' OR ogrenci.sinif='10B') ORDER BY ogrenci.ograd;

	
	 7)Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	
	SELECT ograd,ogrsoyad,atarih FROM ogrenci LEFT JOIN islem ON ogrenci.ogrno=islem.ogrno


	 8)Kitap almayan öğrencileri listeleyin.
	
	SELECT * FROM ogrenci WHERE ogrno NOT IN(SELECT ogrno FROM islem);


	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	

    SELECT islem.kitapno,kitapadi ,COUNT(islem.kitapno)FROM islem INNER JOIN kitap ON kitap.kitapno=islem.kitapno GROUP BY (kitapno) ORDER BY (kitapno)asc;

	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.


    SELECT islem.kitapno,kitapadi, COUNT(islem.kitapno) FROM kitap LEFT JOIN islem ON kitap.kitapno=islem.kitapno GROUP BY(kitapno)


	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	
	SELECT ograd,ogrsoyad,kitap.kitapadi FROM ogrenci INNER JOIN islem ON ogrenci.ogrno=islem.ogrno
    INNER JOIN kitap ON kitap.kitapno=islem.kitapno 
	

	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.


    SELECT ogrenci.ogrno,ograd,ogrsoyad,kitapadi,yazarad,yazarsoyad,turadi,atarih FROM ogrenci 
    LEFT JOIN islem ON ogrenci.ogrno=islem.ogrno
    LEFT JOIN kitap ON islem.kitapno=kitap.kitapno 
    LEFT JOIN yazar ON yazar.yazarno=kitap.yazarno
    LEFT JOIN tur ON tur.turno=kitap.turno ORDER BY ogrenci.ogrno desc;

	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	SELECT ogrenci.ogrno,sinif,ograd,ogrsoyad,COUNT(kitapadi)  FROM ogrenci 
    INNER JOIN islem ON ogrenci.ogrno=islem.ogrno 
    INNER JOIN kitap ON kitap.kitapno=islem.kitapno 
    WHERE ogrenci.sinif LIKE "%10A%" OR ogrenci.sinif LIKE "%10B%"
    GROUP BY(ogrenci.ogrno);
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
	
	SELECT AVG(sayfasayisi)FROM kitap;


	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	

	SELECT kitapadi,sayfasayisi FROM kitap WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap);


	16) Öğrenci tablosundaki öğrenci sayısını gösterin


    SELECT COUNT(ogrno)FROM ogrenci;
	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
 	

    SELECT COUNT(*) AS toplam_sayı_takma FROM ogrenci;

	
    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
    
	SELECT COUNT(DISTINCT ograd) FROM  ogrenci;

	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	

	SELECT MAX(sayfasayisi) FROM kitap;


	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.


	SELECT MAX(sayfasayisi),kitapadi FROM kitap;
	

	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	SELECT MIN(sayfasayisi) FROM kitap;


	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
    
	SELECT MIN(sayfasayisi),kitapadi FROM kitap;

	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	

     SELECT MAX(sayfasayisi),turadi FROM kitap,tur WHERE kitap.turno = tur.turno and turadi="Dram";

	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
	SELECT ogrenci.ogrno, ograd,SUM(kitap.sayfasayisi) FROM ogrenci INNER JOIN  islem  ON ogrenci.ogrno=islem.ogrno
    INNER JOIN kitap ON kitap.kitapno=islem.kitapno
    WHERE ogrenci.ogrno LIKE '%15%'


	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	

    SELECT  ogrenci.ogrno,ograd, COUNT(ograd)  FROM ogrenci GROUP BY ograd


	26) Her sınıftaki öğrenci sayısını bulunuz.
	

    SELECT  sinif, COUNT(ogrenci.ogrno)  FROM ogrenci GROUP BY sinif

	
	 27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
    SELECT sinif, cinsiyet, count(cinsiyet) FROM ogrenci GROUP BY sinif,cinsiyet


	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	

	SELECT ogrenci.ogrno, ograd,ogrsoyad,sayfasayisi,kitapadi, SUM(sayfasayisi) FROM ogrenci
    RIGHT JOIN  islem  ON ogrenci.ogrno=islem.ogrno
    RIGHT JOIN kitap ON kitap.kitapno=islem.kitapno GROUP BY (ogrenci.ogrno)
	ORDER BY toplamSayfaSayisi;


	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
	
	
	SELECT ograd, ogrsoyad, COUNT(kitapadi) AS okunan_kitap_adedi From ogrenci
	INNER JOIN islem ON islem.ogrno = ogrenci.ogrno
	INNER JOIN kitap ON kitap.kitapno = islem.kitapno
	GROUP BY ogrenci.ogrno
	ORDER BY ograd;