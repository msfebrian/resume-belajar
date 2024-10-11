# Contoh Query di AppSheet
## Implementasi di Virtual Column
untuk scan dari row select gunakan fungsi `[_THISROW].`
```
SELECT(kendaraan[BPKB], [ID Barang] = [_THISROW].[ID Barang])
```

```
SELECT(kendaraan[Nomor Polisi], [ID Barang] = "123" AND [Merek] = "Toyota")
```