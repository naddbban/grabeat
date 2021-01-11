﻿## Grab Eat
penerapan MVC untuk memudahkan user dalam pemesanan item.

### SCOPE & FUNCTIONALITIES
- Pemesanan makanan.
- Wadah dalam Keranjang pada class `KeranjangBelanja.cs`
- Kalkulasi perhitungan jumlah Total pada class `Payment.cs`
 

### How does it works?
logika perhitungan untuk melakukan kalkulasi terdapat pad class `Payment.cs`
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