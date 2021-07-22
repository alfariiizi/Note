# Fixing Things

## Fixing microphone external input on Manjaro

Source yang saya gunanakan adalah:
- [Arch Wiki](https://wiki.archlinux.org/title/Advanced_Linux_Sound_Architecture#Correctly_detect_microphone_plugged_in_a_4-pin_3.5mm_(TRRS)_jack)
- [Nobody moving away from SE (superuser stackexchange)](https://superuser.com/questions/1312970/headset-microphone-not-detected-by-pulse-und-alsa#answer-1329331)

Sebelumnya, Install dulu `alsa-tools`:
```zsh
    $ sudo pacman -S alsa-tools
```
Atau kalau kurang mantep, install juga `alsa-utils`:
```zsh
$ sudo pacman -S alsa-utils
```
Tapi seharusnya `alsa-tools` aja udah cukup.

Setelah itu, seperti yang ada pada sumber diatas:
1. Masuk ke directory `/etc/modprobe.d`
2. Cek dengan `ls -a` apakah ada file yang bernama `alsa-base.conf`, jika belum ada maka langsung aja buat sendiri file nya
3. Masuk ke file `alsa-base.conf` tersebut dengan editor `vim` atau `nano`
4. Tambahkan baris baru:
    ```zsh
    options snd_hda_intel index=0 model=<nama-model>
    ```
5. `<nama-model>` diatas diganti dengan model yang sesuai dengan [HD-Audio Codec-Specific Models](https://www.kernel.org/doc/html/latest/sound/hd-audio/models.html#hd-audio-codec-specific-models)
6. Untuk mengetahui kira - kira model sound card (sound type atau apalah terminologi nya) bisa dengan command:
```
    $ cat /proc/asound/card*/codec* | grep Codec
```
7. Dalam kasus saya, saya menggunakan Acer Aspire:
   1. Output step-6:
        ```
        Codec: Realtek ALC256
        Codec: Intel Kabylake HDMI

        ```
   2. Sehingga pada yang harus saya cari adalah model `ALC256`. Dalam section [ALC22x/23x/25x/269/27x/28x/29x (and vendor-specific ALC3xxx models)](https://www.kernel.org/doc/html/latest/sound/hd-audio/models.html#alc22x-23x-25x-269-27x-28x-29x-and-vendor-specific-alc3xxx-models) terdapat model:
        ```md
        aspire-headset-mic
            Headset pin fixup for Acer Aspire
        ```
    3. Maka dari itu, yang saya tulis pada file `alsa-base.conf` adalah:
        ```zsh
        options snd_hda_intel index=0 model=aspire-headset-mic
        ```
8. Reboot komputer
9. Setelah komputer hidup kembali. Pasang headset yang mempunyai mic, kemudian masuk ke `System Settings > Audio`. Ganti *Port* pada *Recording Device* ke *Headset Microphone*.