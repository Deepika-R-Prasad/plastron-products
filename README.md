# Plastron Products - Static Billing System

This is a static HTML billing system that can be hosted on GitHub Pages. It uses CSV files for data storage and JavaScript for all calculations.

## Features

- Generate Tax Invoices
- Generate Proforma Invoices
- View Past Tax Invoices
- View Past Proforma Invoices
- Export invoices to CSV
- Import invoices from CSV
- Print invoices
- Reset & Home functionality on all pages

## Files

- `index.html` - Landing page with options to generate bills, proforma invoices, or view past records
- `generate-bill.html` - First page: Select customer and enter invoice details (shared by both bill and proforma flows)
- `products.html` - Second page: Select products and quantities (shared by both bill and proforma flows)
- `bill-gen.html` - Final page for Tax Invoices: Generate, print, and export bill to CSV
- `proforma-invoice.html` - Final page for Proforma Invoices: Generate, print, and export to CSV
- `past-bills.html` - View past tax invoices by importing from CSV file
- `past-proforma-invoices.html` - View past proforma invoices by importing from CSV file
- `customers.csv` - Customer data file
- `products.csv` - Product data file
- `past-bills.csv` - Past tax invoices data file (manual import/export)
- `past-proforma-invoices.csv` - Past proforma invoices data file (manual import/export)

## How to Use

### Generate New Tax Invoice

1. Open `index.html` in your browser
2. Click "Generate New Bill"
3. Select a customer from the dropdown (populated from customers.csv)
4. Enter invoice details (Tax Invoice #, Purchase Order, DC Number)
5. Check "Add P&F" if packing charges should be applied (1%)
6. Click "Submit" to proceed to product selection
7. On the products page, enter quantities for the products you want to include
8. Click "Save & Generate Bill" to generate the final bill
9. The bill will display with all calculations (subtotal, packing, taxes, grand total)
10. Click "Print" to print the bill
11. Click "Export to CSV" to download the bill as a CSV file
12. Manually add the exported CSV row to `past-bills.csv` file on GitHub

### Generate Proforma Invoice

1. Open `index.html` in your browser
2. Click "Generate Proforma Invoice"
3. Select a customer from the dropdown (populated from customers.csv)
4. Enter proforma invoice details (Proforma Invoice #, Invoice Date, Purchase Order)
5. Note: DC Number and P&F are not applicable for proforma invoices
6. Click "Submit" to proceed to product selection
7. On the products page, enter quantities for the products you want to include
8. Click "Save & Generate Proforma Invoice" to generate the final proforma invoice
9. The proforma invoice will display with all calculations (subtotal, taxes, grand total)
10. Click "Print" to print the proforma invoice
11. Click "Export to CSV" to download the proforma invoice as a CSV file
12. Manually add the exported CSV row to `past-proforma-invoices.csv` file on GitHub

### View Past Tax Invoices

1. Open `index.html` in your browser
2. Click "View Past Bills"
3. Click "Import CSV" to load bills from `past-bills.csv` file
4. Select the `past-bills.csv` file from your computer
5. View the list of past tax invoices
6. Click "View" on any invoice to see it in the exact bill format
7. Click "Print" to print the invoice

### View Past Proforma Invoices

1. Open `index.html` in your browser
2. Click "View Past Proforma Invoices"
3. Click "Import CSV" to load proforma invoices from `past-proforma-invoices.csv` file
4. Select the `past-proforma-invoices.csv` file from your computer
5. View the list of past proforma invoices
6. Click "View" on any invoice to see it in the exact proforma invoice format
7. Click "Print" to print the invoice

### Reset & Home

All pages have a "Reset & Home" button that:
- Clears all cached data from localStorage
- Returns to the index.html page
- Requires confirmation before proceeding

## CSV File Formats

### customers.csv
```
customer_id,customer_name,address_line_one,address_line_two,gstn,contact_number,customer_mail_id,cgst,sgst,igst
```

### products.csv
```
part_id,customer_id,part_name,part_number,code_number,hsn_code,unit_price,charge_type,cgst,sgst,igst
```

### past-bills.csv
```
invoice_number,billing_date,customer_name,customer_address,gstn,contact_number,purchase_order,dc_number,eway_bill,transporter,igst,cgst,sgst,products_json,pre_total,packing,sub_total,igst_amount,cgst_amount,sgst_amount,grand_total,created_at
```

### past-proforma-invoices.csv
```
invoice_number,invoice_date,customer_name,customer_address,gstn,contact_number,purchase_order,igst,cgst,sgst,products_json,sub_total,igst_amount,cgst_amount,sgst_amount,grand_total,created_at
```

**Note:** The `products_json` column contains JSON-encoded product data for each invoice.

## Calculations

The bill calculations follow this logic:
1. **PreTotal**: Sum of (quantity × unit price) for all products
2. **Packing**: If enabled, 1% of PreTotal
3. **SubTotal**: PreTotal + Packing (if enabled)
4. **IGST**: SubTotal × (IGST rate / 100)
5. **CGST**: SubTotal × (CGST rate / 100)
6. **SGST**: SubTotal × (SGST rate / 100)
7. **Grand Total**: Ceiling of (SubTotal + IGST + CGST + SGST)

## Manual Import/Export Workflow

Since browsers cannot write directly to files, the past invoices system uses manual import/export:

### For Tax Invoices:
1. **Export**: After generating a tax invoice, click "Export to CSV" in bill-gen.html
2. **Manual Update**: Add the exported CSV row to `past-bills.csv` file
3. **Upload**: Commit and push the updated `past-bills.csv` to GitHub
4. **Import**: In past-bills.html, click "Import CSV" to load the updated file

### For Proforma Invoices:
1. **Export**: After generating a proforma invoice, click "Export to CSV" in proforma-invoice.html
2. **Manual Update**: Add the exported CSV row to `past-proforma-invoices.csv` file
3. **Upload**: Commit and push the updated `past-proforma-invoices.csv` to GitHub
4. **Import**: In past-proforma-invoices.html, click "Import CSV" to load the updated file

This approach allows:
- Data sharing across users via GitHub
- Backup and version control of past invoices
- No server-side code required

## Data Security

- No data is sent to any server
- All processing happens client-side
- Past tax invoices are stored in `past-bills.csv` on GitHub (manual export/import)
- Past proforma invoices are stored in `past-proforma-invoices.csv` on GitHub (manual export/import)
- Temporary form data is stored in localStorage and cleared after use
- Reset & Home button clears all cached data

## Deployment for GitHub Pages

### Deploy to GitHub Pages

1. Upload all files in the `static` folder to your GitHub repository
2. Enable GitHub Pages in your repository settings
3. Select the branch and folder where the files are located
4. Access your site at `https://yourusername.github.io/repository-name/`

**Note:** The application requires CSV files (customers.csv, products.csv, past-bills.csv, past-proforma-invoices.csv) to be present in the same directory as the HTML files.

## Browser Compatibility

Works in all modern browsers that support:
- Fetch API
- localStorage
- FileReader API
- ES6 JavaScript

## Customization

To add new customers or products:
1. Edit the respective CSV file
2. Follow the format shown in the existing files
3. Save the file
4. The changes will be reflected immediately when you refresh the page
