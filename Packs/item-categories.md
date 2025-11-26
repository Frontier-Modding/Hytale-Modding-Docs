# Getting started with Item Categories

- Item Categories are the Tabs in your Creative Menu
- You can create your own Item Categories and also sub Item Categories

1. `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Server/Item/Category/CreativeLibrary/MyItemCategory.json`

- This is the main json where you configure your new Item Category

2. Inside the json it should look like this:

```{
  "Icon": "Icons/ItemCategories/MyCategory.png",
  "Order": 6,
  "Children": [
    {
      "Id": "Example_Category",
      "Name": "ui.itemcategory.example_category",
      "Icon": "Icons/ItemCategories/My_Example_Category.png"
    },
	{
      "Id": "Example_Category_Two",
      "Name": "ui.itemcategory.example_category_two",
      "Icon": "Icons/ItemCategories/My_Example_Category_Two.png"
    }
  ]
}```
	
- Icon are the icons in the Creative Menu for your Item Categories
- Order is the order of your Item Category in the GUI
- Children are your "sub" Item Categories
- Name is the text when you hover your mouse over the Item Category icon

3. Add your translation into:
 `YourPackName/Server/Languages/en-US/ui.lang`
 
 itemcategory.example_category = Example Tab One
 itemcategory.example_category_two = Example Tab Two
 
4. Add your icons into
 `YourPackName/Common/Icons/ItemCategories/MyCategory.png`
  
5. Adding blocks into your tab by editing the "blockstate" json inside
 `YourPackName/Server/Item/Items/your_block.json`
 
 Just include this inside your block json
   "Categories": [
    "MyCategory.Example_Category"
  ],
  
  MyCategory is the main tab
  Example_Category is the sub tab
  
6. For the Vanilla category names look into
 `.Assets/Server/Item/Category/CreativeLibrary/`
