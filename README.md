İLK ÖNCE BENİ İNDİRDİĞİN İÇİN TEŞŞEKÜRLER 

KURULUM

VSCODEYİ İNDİRMEN GEREKLİ 
VE YENİ PROJE YAP

VE ŞU TUŞLARA BAS
CTRL SHİFT X

ARAMA YERİNE PYTHON YAZ MİCROSOFT OLANLARI İNDİR 3 TANE PYTHON OLACAKTIR MİCROSOFT DEN YAPILANLARI İNDİR 
ŞİMDİ KODLARI VERİYORUM ÇALIŞTIRMAK İÇİN VİSUAL STUDİO CODEYE GİDİP CTRL F5 E BASMAN YETERLİ 

KOD:import pygame
import time
import random

pygame.init()

# Renkler
beyaz = (255, 255, 255)
yeşil = (0, 255, 0)
kırmızı = (213, 50, 80)
mavi = (50, 153, 213)
siyah = (0, 0, 0)

# Ekran boyutu
genişlik = 600
yükseklik = 400  # Burada 'yüksekliği' yerine 'yükseklik' kullanılmalıydı

# Pencere
pencere = pygame.display.set_mode((genişlik, yükseklik))  # Buradaki 'yüksekliği' de düzeltildi
pygame.display.set_caption("Yılan Oyunu")

# Yılan ayarları
yılan_blok = 10
yılan_hız = 15

# Saat
saat = pygame.time.Clock()

# Fontlar
font_stil = pygame.font.SysFont("bahnschrift", 25)
puan_font = pygame.font.SysFont("comicsansms", 35)

def puan_göster(puan):
    value = puan_font.render("Puan: " + str(puan), True, beyaz)
    pencere.blit(value, [0, 0])

def yılan(yılan_blok, yılan_listesi):
    for x in yılan_listesi:
        pygame.draw.rect(pencere, yeşil, [x[0], x[1], yılan_blok, yılan_blok])

def oyun_bitti(puan):
    mesaj = font_stil.render("Oyun Bitti! Puanınız: " + str(puan), True, kırmızı)
    pencere.blit(mesaj, [genişlik / 6, yükseklik / 3])
    pygame.display.update()
    time.sleep(2)

def oyun():
    oyun_bitti_flag = False
    oyun_bitti_puan = 0

    x1 = genişlik / 2
    y1 = yükseklik / 2

    x1_hız = 0
    y1_hız = 0

    yılan_listesi = []
    uzunluk = 1

    # Yiyecek yerini rastgele seç
    yiyecekx = round(random.randrange(0, genişlik - yılan_blok) / 10.0) * 10.0
    yiyeceky = round(random.randrange(0, yükseklik - yılan_blok) / 10.0) * 10.0

    while not oyun_bitti_flag:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                oyun_bitti_flag = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_hız = -yılan_blok
                    y1_hız = 0
                elif event.key == pygame.K_RIGHT:
                    x1_hız = yılan_blok
                    y1_hız = 0
                elif event.key == pygame.K_UP:
                    y1_hız = -yılan_blok
                    x1_hız = 0
                elif event.key == pygame.K_DOWN:
                    y1_hız = yılan_blok
                    x1_hız = 0

        if x1 >= genişlik or x1 < 0 or y1 >= yükseklik or y1 < 0:
            oyun_bitti_flag = True

        x1 += x1_hız
        y1 += y1_hız
        pencere.fill(mavi)

        pygame.draw.rect(pencere, kırmızı, [yiyecekx, yiyeceky, yılan_blok, yılan_blok])

        yılan_baş = []
        yılan_baş.append(x1)
        yılan_baş.append(y1)
        yılan_listesi.append(yılan_baş)

        if len(yılan_listesi) > uzunluk:
            del yılan_listesi[0]

        for x in yılan_listesi[:-1]:
            if x == yılan_baş:
                oyun_bitti_flag = True

        yılan(yılan_blok, yılan_listesi)
        puan_göster(uzunluk - 1)

        pygame.display.update()

        if x1 == yiyecekx and y1 == yiyeceky:
            yiyecekx = round(random.randrange(0, genişlik - yılan_blok) / 10.0) * 10.0
            yiyeceky = round(random.randrange(0, yükseklik - yılan_blok) / 10.0) * 10.0
            uzunluk += 1

        saat.tick(yılan_hız)

    oyun_bitti(uzunluk - 1)

    # Kullanıcıdan bir tuşa basmasını bekliyoruz, böylece pencere kapanmaz.
    input("Oyunun bitmesiyle pencereyi kapatmak için Enter'a basın...")

    pygame.quit()
    quit()

# Oyunu başlat
oyun()
