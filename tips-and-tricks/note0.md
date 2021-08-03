# Tips and Tricks

## How to Link to a Specific Part Of A Webpage & Share it

[link](https://techwiser.com/specific-part-of-a-webpage/)

## Nested list in Markdown

[link](https://stackoverflow.com/questions/37575916/how-to-markdown-nested-list-items-in-bitbucket#answer-37594372)

## Newline in Markdown

Use `<br />` to create breakline or newline.

## Snap Package Problem

Beberapa hari yang lalu (Akhir juli 2021) vscode tidak bisa dibuka dikarenakan ada error[^error-open-vscode]. Setelah itu, pada saya coba uninstall dan kemudian menginstall [vscode-oss][vscode-oss]. Akan tetapi, extensions pada [vscode-oss][vscode-oss] entah mengapa tidak mau connect ke internet. Katanya _"no connection, please try later"_ kurang lebih begitu. Saya coba cari cari di stackoverflow, kemudian ketemu [ini][ext-issue-sof]. Tetapi, saya kurang yakin dengan solusi tersebut, dikarenakan terdapat beberapa issue di github [vscode-oss][vscode-oss] pada bulan juli 2021 yang juga mengatakan kalau [vscode-oss][vscode-oss] tidak bisa extension marketplace nya. Sayapun menyerah kemudian saya coba install vscode lagi melalui flatpak. Tetapi ternyata ada beberapa software yang belum saya update pada flatpak sehingga membuat saya harus mengupdate nya lebih dahulu. Karena males nunggu update nya, saya putuskan untuk menggunakan _snap_ lagi.

Pada saat menginstall menggunakan _snap_, terdapat error seperti berikut:

```plain
error: cannot install "code": classic confinement requires snaps under /snap or symlink from /snap
       to /var/lib/snapd/snap
```

Issue tersebut dapat diselesaikan dengan solusi pada stackoverflow [ini][simlink-snap].

[vscode-oss]: https://github.com/microsoft/vscode/wiki/Differences-between-the-repository-and-Visual-Studio-Code
[ext-issue-sof]: https://stackoverflow.com/questions/64463768/cant-find-certain-extensions-in-code-ossopen-source-variant-of-visual-studio-c
[simlink-snap]: https://stackoverflow.com/questions/68565756/cannot-install-code-classic-confinement-requires-snaps-under-snap-or-symlink#answer-68579520

## Different between Flatpak and Snap

[Manjaro Forum](https://forum.manjaro.org/t/snap-vs-flatpak-vs-system-packages/28711)

[FOSS](https://www.fosslinux.com/42410/snap-vs-flatpak-vs-appimage-know-the-differences-which-is-better.htm)

Summary:

- Flatpak: Decentralized, build by Red Hat.
- Snap: Centralized, held by Canonical.

## Unicast Address

- <https://citraweb.com/artikel_lihat.php?id=232>
	: Memberikan penjelasan yang cukup bagus mengenai_Address Type_ (Unicast, Multicast, Anycast, Broadcast), namun kurang memberikan penjelasan penggunaan dan contoh nya.
- <https://bradhedlund.com/2007/11/21/identifying-ethernet-multicast/>
	: Memberikan penjelasan yang cukup baik, namun saya masih belum paham 😅 (lihat point selanjutnya biar lebih paham).
- <https://www.edureka.co/community/37551/changing-address-siocsifhwaddr-assign-requested-address#a37553>
	: MAC address harus pakek **Unicast Address**, sehingga first octet nya harus **genap**. Contoh nya adalah `12:22:33:44:55:66`, dimana `12` adalah genap. Sehingga kalau disimpulkan, angka yang dapat digunakan di first octet di word[^word] sebelah kanan adalah `0`, `2`, `4`, `6`, `8`, `A`, `C`, dan `E`.

## END

[^error-open-vscode]: Jika dibuka langsung di aplikasi nya maka tidak ada laporan error. Namun jika dibuka melalui terminal dengan command `code`, maka akan terdapat laporan error.

[^word]: $1\ word \equiv 4\ bit$