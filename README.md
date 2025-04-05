import java.util.Scanner;

public class SinemaSistemi {
    static String[] filmler = new String[10];
    static String[] filmTurleri = new String[10];
    static int[] filmSureleri = new int[10];
    static int filmSayisi = 0;

    static String[] musteriler = new String[20];
    static String[] musteriMailleri = new String[20];
    static int musteriSayisi = 0;

    static boolean[][] biletler = new boolean[20][10];

    static Scanner input = new Scanner(System.in);

    public static void main(String[] args) {
        int secim;

        do {
            System.out.println("\n--- SINEMA MUSTERI KAYIT SISTEMI ---");
            System.out.println("1. Film Ekle");
            System.out.println("2. Filmleri Listele");
            System.out.println("3. Musteri Ekle");
            System.out.println("4. Musterileri Listele");
            System.out.println("5. Bilet Olustur");
            System.out.println("6. Biletleri Listele");
            System.out.println("0. Cikis");
            System.out.print("Seciminiz: ");
            secim = input.nextInt();
            input.nextLine(); // dummy enter

            switch (secim) {
                case 1:
                    filmEkle();
                    break;
                case 2:
                    filmListele();
                    break;
                case 3:
                    musteriEkle();
                    break;
                case 4:
                    musteriListele();
                    break;
                case 5:
                    biletOlustur();
                    break;
                case 6:
                    biletListele();
                    break;
                case 0:
                    System.out.println("Programdan cikiliyor...");
                    break;
                default:
                    System.out.println("Gecersiz secim!");
            }
        } while (secim != 0);
    }

    static void filmEkle() {
        if (filmSayisi >= 10) {
            System.out.println("Maksimum film sayisina ulasildi.");
            return;
        }

        System.out.print("Film adi: ");
        filmler[filmSayisi] = input.nextLine();

        System.out.print("Film suresi (dk): ");
        filmSureleri[filmSayisi] = input.nextInt();
        input.nextLine();

        System.out.print("Film turu: ");
        filmTurleri[filmSayisi] = input.nextLine();

        filmSayisi++;
        System.out.println("Film eklendi.");
    }

    static void filmListele() {
        if (filmSayisi == 0) {
            System.out.println("Henuz film eklenmemis.");
            return;
        }

        System.out.println("--- Filmler ---");
        for (int i = 0; i < filmSayisi; i++) {
            System.out.println(i + " - " + filmler[i] + " (" + filmSureleri[i] + " dk, " + filmTurleri[i] + ")");
        }
    }

    static void musteriEkle() {
        if (musteriSayisi >= 20) {
            System.out.println("Maksimum musteri sayisina ulasildi.");
            return;
        }

        System.out.print("Musteri adi: ");
        musteriler[musteriSayisi] = input.nextLine();

        System.out.print("Email: ");
        musteriMailleri[musteriSayisi] = input.nextLine();

        musteriSayisi++;
        System.out.println("Musteri eklendi.");
    }

    static void musteriListele() {
        if (musteriSayisi == 0) {
            System.out.println("Henuz musteri eklenmemis.");
            return;
        }

        System.out.println("--- Musteriler ---");
        for (int i = 0; i < musteriSayisi; i++) {
            System.out.println(i + " - " + musteriler[i] + " (" + musteriMailleri[i] + ")");
        }
    }

    static void biletOlustur() {
        musteriListele();
        System.out.print("Musteri numarasi: ");
        int mIndex = input.nextInt();

        filmListele();
        System.out.print("Film numarasi: ");
        int fIndex = input.nextInt();

        if (mIndex < musteriSayisi && fIndex < filmSayisi) {
            biletler[mIndex][fIndex] = true;
            System.out.println("Bilet olusturuldu: " + musteriler[mIndex] + " -> " + filmler[fIndex]);
        } else {
            System.out.println("Gecersiz musteri veya film secimi.");
        }
    }

    static void biletListele() {
        System.out.println("--- Biletler ---");
        for (int i = 0; i < musteriSayisi; i++) {
            for (int j = 0; j < filmSayisi; j++) {
                if (biletler[i][j]) {
                    System.out.println(musteriler[i] + " -> " + filmler[j]);
                }
            }
        }
    }
}
