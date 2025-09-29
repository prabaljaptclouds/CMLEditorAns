type LineItem;


type Laptop : LineItem {
    @(defaultValue = "1080p Built-in Display")
    string Display = ["4k Built-in Display", "1080p Built-in Display", "2k Built-in Display"];

    @(defaultValue = "13 Inch")
    string Screen_Size = ["27 Inch", "13 Inch", "15 Inch", "24 Inch"];

    string Graphics = ["Intel Iris Xe Graphics", "MSI Gaming GeForce RTX 3060"];

    @(defaultValue = "SSD Hard Drive 256GB")
    string Storage = ["Cloud Storage Enterprise - 2 TB", "SSD Hard Drive 1TB", "SSD Hard Drive 256GB", "SSD Hard Drive 512GB", "SSD Hard Drive 2TB", "Cloud Storage Enterprise - 6 TB"];

    @(defaultValue = "i5-CPU 4.4GHz")
    string Processor = ["Intel Core i9 5.2 GHz", "i7-CPU 4.7GHz", "i5-CPU 4.4GHz"];

    @(defaultValue = "RAM 8GB")
    string Memory = ["RAM 16GB", "RAM 64GB", "RAM 32GB", "RAM 8GB"];

    @(defaultValue = "FALSE")
    boolean Wireless;

}

type LaptopBasicBundle : LineItem {
    relation laptop : Laptop;

    relation antivirus : Antivirus;

    string Storage = ["Cloud Storage Enterprise - 2 TB", "SSD Hard Drive 1TB", "SSD Hard Drive 256GB", "SSD Hard Drive 512GB", "SSD Hard Drive 2TB", "Cloud Storage Enterprise - 6 TB"];

}

type Antivirus : LineItem;

type LaptopProBundle : LineItem {
   
    relation laptop1 : Laptop[1..9999];

    relation warranty : Warranty;

    relation printerbundle : PrinterBundle;

    relation mouse : Mouse;

    int ProQuanitity = this.quantity;

}

type Warranty : LineItem;

type PrinterBundle : LineItem {
    relation printer : Printer[1..9999];

    relation printerpaper : PrinterPaper;

}

type Printer : LineItem {
    string Printer_Type = ["Laser", "Inkjet"];

}

type PrinterPaper : LineItem {
    string Paper_Size = ["Letter", "A4"];

}

type Mouse : LineItem {
    @(defaultValue = "false")
    boolean Wireless;

}

@(virtual = true)
type Quote {
    @(sourceContextNode = "SalesTransaction.SalesTransactionItem")
    relation laptopProBundle : LaptopProBundle;

    int laptopProquantity = laptopProBundle.ProQuanitity;
    @(sourceContextNode = "SalesTransaction.SalesTransactionItem")
    relation laptopBasicBundle : LaptopBasicBundle;

    constraint(laptopProquantity> 5 && laptopBasicBundle.Storage == 'Cloud Storage Enterprise - 6 TB' -> laptopBasicBundle[LaptopBasicBundle].laptop[Laptop]);

}
