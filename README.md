# Nama  : Muhammad Daffa Rafikanza Noor
# NIM   : 19.11.2826

# Studi Kasus

Aplikasi ini bertujuan simulasi pembelian makanan dan minuman dengan menggunakan promo/voucher.

## Scope and Functionalities

- User dapat melihat daftar makanan dan minuman
- User dapat melihat voucher yang ditawarkan
- User bisa menggunakan voucher untuk diskon makanan dan minuman yang dipesan
- User dapat menghapus pesanan yang udah di pesan
- User dapat melihat potongan harga dengan menggunakan salah satu voucher yang tersedia

## How Does It Works?

Dalam kasus yang diberikan oleh saya, ada 4 buah model dan 3 buah controller yang bertujuan untuk menjalankan program ini.

Dimulai dari `Penawaran.xaml.cs` yang bertujuan untuk menawarkan list makanan yang bisa digunakan sebuah voucher untuk ditampilkan di listbox.

```csharp
private void generateContentPenawaran()
        {
            Item coffeLate = new Item("Coffe Late", 30000);
            Item blackTea = new Item("BlackTea", 20000);
            Item milkShake = new Item("Milk Shake", 15000);
            Item watermelonJuice = new Item("Watermelon Juice", 25000);
            Item lemonSquash = new Item("Lemon Squash", 30000);
            Item pizza = new Item("Pizza", 75000);
            Item friedRice = new Item("Fried Rice Special", 45000);

            Penawarancontroller.addItem(coffeLate);
            Penawarancontroller.addItem(blackTea);
            Penawarancontroller.addItem(milkShake);
            Penawarancontroller.addItem(watermelonJuice);
            Penawarancontroller.addItem(lemonSquash);
            Penawarancontroller.addItem(pizza);
            Penawarancontroller.addItem(friedRice);

            listPenawaran.Items.Refresh();
        }
```

Kemudian pada `PilihVoucher.xaml.cs` yang bertujuan untuk mengsingkronisasi voucher dan model penawaran yang akan ditampilkan pada listbox juga.

```csharp
private void generateListVoucher()
        {
            Model.Voucher awalTahun = new Model.Voucher(title: "Promo Awal Tahun Diskon 25%", discInPercent: 25);
            Model.Voucher tebusMurah = new Model.Voucher(title: "Promo Tebus Murah Diskon 30% atau max. 30.000", discInPercent: 30);
            Model.Voucher promoNatal = new Model.Voucher(title: "Promo Natal Potongan 10000", disc: 10000);

            voucherController.addItem(awalTahun);
            voucherController.addItem(tebusMurah);
            voucherController.addItem(promoNatal);

            DaftarVoucher.Items.Refresh();
        }
```

kemudian pada `MainWindow.xaml.cs`kita menginisiasi object dari `Penawaran.xaml.cs` dan juga `Voucher.xaml.cs`, untuk di masukkan dalam sebuah list KeranjangBelanja dan menampilkan total dari semua yang user belanja, akan ditampilkan di listbox.

```csharp
public partial class MainWindow : Window,
        OnPenawaranChangedListener,
        OnPilihVoucherChangedListener,
        OnPaymentChangedListener,
        OnKeranjangBelanjaChangedListener
    {
        MainWindowController controller;
        Payment payment;

        public MainWindow()
        {
            InitializeComponent();

            payment = new Payment(this);
            payment.setBalance(500000);
            payment.setPromo(0);

            KeranjangBelanja keranjangBelanja = new KeranjangBelanja(payment, this);

            controller = new MainWindowController(keranjangBelanja);

            listBoxPesanan.ItemsSource = controller.getSelectedItems();
            listBoxPakaiVoucher.ItemsSource = controller.getSelectedVouchers();

            initializeView();

        }

        private void initializeView()
        {
            labelSubtotal.Content = 0;
            labelGrantTotal.Content = 0;
            labelPromoFee.Content = (payment.getPromo() > 0) ? -payment.getPromo() : 0;
        }

        public void onPenawaranSelected(Item item)
        {
            controller.addItem(item);
        }

        private void onButtonAddItemClicked(object sender, RoutedEventArgs e)
        {
            Penawaran penawaranWindow = new Penawaran();
            penawaranWindow.SetOnItemSelectedListener(this);
            penawaranWindow.Show();
        }

        private void listBoxPesanan_ItemClicked(object sender, MouseButtonEventArgs e)
        {
            if (MessageBox.Show("Kamu ingin menghapus item ini?",
                    "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                ListBox listBox = sender as ListBox;
                Item item = listBox.SelectedItem as Item;
                controller.deleteSelectedItem(item);
            }
        }

        private void listBoxPakaiVoucher_ItemClicked(object sender, MouseButtonEventArgs e)
        {
            if (MessageBox.Show("Kamu ingin membatalkan voucher ini?",
                   "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                ListBox listBox = sender as ListBox;
                Voucher item = listBox.SelectedItem as Voucher;
                controller.deleteSelectedVoucher(item);
            }
        }
``` 
