﻿## Grab Eat
penerapan MVC untuk memudahkan user dalam pemesanan item.

### SCOPE & FUNCTIONALITIES
- Pemesanan makanan.
- Wadah dalam Keranjang pada class `KeranjangBelanja.cs`
- Kalkulasi perhitungan jumlah Total pada class `Payment.cs`
 

### How does it works?
User memilih makanan pada `MENU` button, kemudian pada pop up Window user bisa memilih menu yg tersedia dengan cara `Double Click` atau `Select Item`
pada pop up Window `MENU` akan di Bind ke dalam ListBox pada `MainWindow` berupa nama Item dan Harga.

User dapat memilih voucher pada `Voucher` button, saat muncul pop up Window user dapah memilih voucher yang tersedia.

User dapat melihat Subtotal sebelum dikurangi dengan voucher.

User dapat melihat Total keseluruhan setelah dikurang dengan voucher atau tanpa voucher.

kalkulasi perhitungan subtotal pada class `KeranjangBelanja.cs`
```csharp
private void calculateSubTotal()
        {
            double subTotal = 0;
            double promo = 0;
            foreach (Item item in itemkeranjangBelanja)
            {
                subTotal += item.price;

            }
            foreach (Diskon diskon in diskonDipakai)
            {
                if (diskon.potongan == 10000)
                {
                    promo = 10000;
                }
                else if (diskon.potongan == 30000)
                {

                    promo = (subTotal * 30 / 100);

                    if (promo > 30000)
                    {
                        promo = 30000;
                    }
                    else
                    {
                        promo = (subTotal * 30 / 100);
                    }

                }
                else if (diskon.potongan == 25000)
                {
                    promo = (subTotal * 25 / 100);

                }

            }

            payment.updateTotal(subTotal, promo);
        }
```

kalkulasi perhitungan jumlah Total pada class `Payment.cs`
``` csharp
class Payment
    {
        OnPaymentChangedListener paymentListener;
        public Payment(OnPaymentChangedListener paymentListener)
        {

            this.paymentListener = paymentListener;
        }

        public void updateTotal(double subTotal, double promo)
        {

            double total = subTotal - promo;
            this.paymentListener.onPriceUpdated(subTotal, total, promo);

        }
    }
```
