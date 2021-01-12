## Grab Eat
penerapan MVC untuk memudahkan user dalam pemesanan item.

## SCOPE & FUNCTIONALITIES
- Pemesanan makanan.
- Wadah dalam Keranjang pada class `KeranjangBelanja.cs`
- Kalkulasi perhitungan jumlah Total pada class `Payment.cs`
 

### How does it works?
User memilih makanan pada `MENU` button, kemudian pada pop up Window user bisa memilih menu yg tersedia dengan cara `Double Click` atau `Select Item`
pada pop up Window `MENU` akan di Bind ke dalam ListBox pada `MainWindow` berupa nama Item dan Harga. 

berikut cara data bind dari Lisbox `Menu` sinkron dengan Listbox pada `Mainwindow` untuk melakukan perubah setiap terjadi perubahan data.

code dari Menu
```xaml
<ListBox x:Name="listMenu" MouseDoubleClick="listMenuOnDoubleClicked" HorizontalAlignment="Left" Width="360" Grid.ColumnSpan="2">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <Grid Margin="0,2">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="300" />
                        </Grid.ColumnDefinitions>
                        <TextBlock Grid.Row="0" Text="{Binding item}" TextAlignment="Left" FontSize="16"/>
                        <TextBlock Grid.Row="0" Text="{Binding price}"  TextAlignment="Right" FontSize="16" Width="250"/>
                    </Grid>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
```

```csharp
 private void listMenuOnDoubleClicked(object sender, MouseButtonEventArgs e)
        {
            ListBox listbox = sender as ListBox;
            Item item = listbox.SelectedItem as Item;
            this.listener.OnMenuSelected(item);


        }
```

ke MainWnidow
```xaml
<ListBox x:Name="listKeranjangBelanja" HorizontalAlignment="Left" Height="236" VerticalAlignment="Top" Width="366" Margin="169,66,0,0" MouseDoubleClick="onlistKeranjangBelanjaDoubleClicked" Grid.Column="1" Grid.RowSpan="2" Grid.ColumnSpan="3">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <Grid Margin="0,2">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="350" />

                        </Grid.ColumnDefinitions>
                        <TextBlock Grid.Row="0" Text="{Binding item}" TextAlignment="Left" Width="315"/>
                        <TextBlock Grid.Row="0" Text="{Binding price}"  TextAlignment="Right" Width="280"/>
                    </Grid>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
```
menghapus data yg telah di bind
```csharp
 private void onlistKeranjangBelanjaDoubleClicked(object sender, MouseButtonEventArgs e)
        {
            if (MessageBox.Show("Kamu ingin menghapus item ini?",
                   "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                ListBox listBox = sender as ListBox;
                Item item = listBox.SelectedItem as Item;
                controller.removeItem(item);
            }
        }
```        

User dapat memilih voucher pada `Voucher` button, saat muncul pop up Window user dapat memilih voucher yang tersedia. menggunakan data binding
dengan cara kerja yang sama seperti pada Menu ke MainWindow perbedannya hanya promo tidak dapat di hapus, hanya bisa merubah promo yg akan dipakai.

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

## ScreenShot

<p align="center" width="100%">
 MainWindow awal
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="MainWindow awal" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss1.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>
 
 <p align="center" width="100%">
 Pop up Menu Window
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="Pop up Menu Window" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss2.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>

<p align="center" width="100%">
 Get item
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="Get item" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss3.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>

<p align="center" width="100%">
 Pop up Voucher Window
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="Pop up Voucher Window" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss4.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>

<p align="center" width="100%">
 Get Voucher
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="Get Voucher" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss5.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>

<p align="center" width="100%">
 View Total after get Item and Voucher 
 </p>
 <br>
  <p align="center" width="100%">
  <img alt="View Total after get Item and Voucher" src="https://raw.githubusercontent.com/naddbban/grabeat/master/grabeat/Screenshot/ss6.png" style="display:block;float:none;margin-left:auto;margin-right:auto;width:100%"/>
</p>


by Muhammad Husni Anas 19.11.2845
