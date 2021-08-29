1.Find all the information about each products:
**db.product.find().pretty();**

2. Find the product price which are between 400 to 800:
**db.product.find({product_price:{$gte:400,$lte:800}});**

3. Find the product price which are not between 400 to 600:
**db.product.find({product_price:{$not:{$gte:400,$lte:600}}});**

4. List the four product which are greater than 500 in price:
**db.product.find({product_price:{$gt:500}}).limit(4);**

5. Find the product name and product material of each products:
**db.product.find({},{product_name:1,product_material:1});**

6. Find the product with a row id of 10:
**db.product.find({"id":"10"});**

7. Find only the product name and product material:
**db.product.find({},{product_name:1,product_material:1,_id:0});**

8. Find all products which contain the value of soft in product material:
**db.product.find({"product_material":"Soft"});**

9. Find products which contain product color indigo and product price 492.00:
**db.product.find({$and:[{"product_color":"indigo","product_price":492.00}]});**

10. Delete the products which product price value are same:
**var duplicates = [];
db.product.aggregate([
  { $match: { 
    product_price:{ "$ne": "" } 
  }},
  { $group: { 
    _id: { product_price:"$product_price"},
    dups: { "$addToSet": "$product_price" }, 
    count: { "$sum": 1 } 
  }}, 
  { $match: { 
    count: { "$gt": 1 }
  }}
],
{allowDiskUse: true}     
).forEach(function(doc) {
        doc.dups.forEach(function(dupId){ 
        duplicates.push(dupId);  
        }
    )    
})
printjson(duplicates);
result:[36,47]
db.product.remove({product_price:{$in:duplicates}} 
result: WriteResult({ "nRemoved" : 4 })**
