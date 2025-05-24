import java.io.*;
import java.util.*;

// Abstract class for common Entity behavior
abstract class BaseEntity {
    protected UUID id;

    public BaseEntity() {
        this.id = UUID.randomUUID();
    }

    public UUID getId() {
        return id;
    }
}

class Ucak extends BaseEntity {
    private String model;
    private String marka;
    private String seriNo;
    private int koltukKapasitesi;

    public Ucak(String model, String marka, String seriNo, int koltukKapasitesi) {
        super();
        this.model = model;
        this.marka = marka;
        this.seriNo = seriNo;
        this.koltukKapasitesi = koltukKapasitesi;
    }

    public int getKoltukKapasitesi() {
        return koltukKapasitesi;
    }

    public String toString() {
        return String.format("%s - %s (%s), Kapasite: %d", marka, model, seriNo, koltukKapasitesi);
    }
}

class Lokasyon extends BaseEntity {
    private String ulke;
    private String sehir;
    private String havaalani;
    private boolean aktif;

    public Lokasyon(String ulke, String sehir, String havaalani, boolean aktif) {
        this.ulke = ulke;
        this.sehir = sehir;
        this.havaalani = havaalani;
        this.aktif = aktif;
    }

    public String toString() {
        return String.format("%s, %s - %s (%s)", sehir, ulke, havaalani, aktif ? "Aktif" : "Pasif");
    }
}

class Ucus extends BaseEntity {
    private Lokasyon lokasyon;
    private String saat;
    private Ucak ucak;
    private List<Rezervasyon> rezervasyonlar;

    public Ucus(Lokasyon lokasyon, String saat, Ucak ucak) {
        this.lokasyon = lokasyon;
        this.saat = saat;
        this.ucak = ucak;
        this.rezervasyonlar = new ArrayList<>();
    }

    public boolean rezervasyonYap(Rezervasyon r) {
        if (rezervasyonlar.size() < ucak.getKoltukKapasitesi()) {
            rezervasyonlar.add(r);
            return true;
        }
        return false;
    }

    public String toString() {
        return String.format("Uçuş: %s, Saat: %s, Uçak: %s", lokasyon.toString(), saat, ucak.toString());
    }
}

class Rezervasyon extends BaseEntity {
    private Ucus ucus;
    private String ad;
    private String soyad;
    private int yas;

    public Rezervasyon(Ucus ucus, String ad, String soyad, int yas) {
        this.ucus = ucus;
        this.ad = ad;
        this.soyad = soyad;
        this.yas = yas;
    }

    public String toCSV() {
        return String.format("%s,%s,%s,%d,%s", ad, soyad, ucus.toString(), yas, getId());
    }
}

public class UcakRezervasyonApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        Ucak ucak1 = new Ucak("A320", "Airbus", "SN123", 2);
        Lokasyon lokasyon1 = new Lokasyon("Türkiye", "İstanbul", "IST", true);
        Ucus ucus1 = new Ucus(lokasyon1, "10:00", ucak1);

        System.out.println("Uçuş Bilgisi:");
        System.out.println(ucus1);

        for (int i = 0; i < 3; i++) {
            System.out.println("\nRezervasyon Yapmak İçin Bilgilerinizi Giriniz:");
            System.out.print("Ad: ");
            String ad = scanner.nextLine();
            System.out.print("Soyad: ");
            String soyad = scanner.nextLine();
            System.out.print("Yaş: ");
            int yas = Integer.parseInt(scanner.nextLine());

            Rezervasyon r = new Rezervasyon(ucus1, ad, soyad, yas);

            if (ucus1.rezervasyonYap(r)) {
                kaydetRezervasyon(r);
                System.out.println("Rezervasyon başarıyla yapıldı!");
            } else {
                System.out.println("Üzgünüz, uçakta boş koltuk kalmadı.");
            }
        }

        scanner.close();
    }

    public static void kaydetRezervasyon(Rezervasyon r) {
        try (FileWriter fw = new FileWriter("rezervasyonlar.csv", true)) {
            fw.write(r.toCSV() + "\n");
        } catch (IOException e) {
            System.out.println("Dosyaya yazma hatası: " + e.getMessage());
        }
    }
}
