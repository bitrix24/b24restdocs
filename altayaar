// Flutter app: Kash Market (Basic E-commerce)

import 'package:flutter/material.dart';

void main() => runApp(KashMarketApp());

class KashMarketApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Kash Market',
      theme: ThemeData(primarySwatch: Colors.green),
      home: CategoriesPage(),
    );
  }
}

class CategoriesPage extends StatelessWidget {
  final categories = ['ملابس', 'إلكترونيات', 'أحذية', 'اكسسوارات'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('كاش ماركت')),
      body: ListView.builder(
        itemCount: categories.length,
        itemBuilder: (context, index) => ListTile(
          title: Text(categories[index]),
          trailing: Icon(Icons.arrow_forward_ios),
          onTap: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (_) => ProductListPage(category: categories[index]),
              ),
            );
          },
        ),
      ),
    );
  }
}

class ProductListPage extends StatelessWidget {
  final String category;

  ProductListPage({required this.category});

  final products = [
    {'name': 'تي شيرت', 'price': '100 EGP'},
    {'name': 'سماعات بلوتوث', 'price': '250 EGP'},
    {'name': 'حذاء رياضي', 'price': '300 EGP'}
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('منتجات: $category')),
      body: ListView(
        children: products.map((product) {
          return ListTile(
            title: Text(product['name']!),
            subtitle: Text(product['price']!),
            trailing: Icon(Icons.shopping_cart_outlined),
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (_) => ProductDetailPage(product: product),
                ),
              );
            },
          );
        }).toList(),
      ),
    );
  }
}

class ProductDetailPage extends StatelessWidget {
  final Map<String, String> product;

  ProductDetailPage({required this.product});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(product['name']!)),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(product['name']!, style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
            SizedBox(height: 10),
            Text(product['price']!, style: TextStyle(fontSize: 18)),
            SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('تمت إضافة المنتج إلى السلة')),
                );
              },
              child: Text('إضافة إلى السلة'),
            )
          ],
        ),
      ),
    );
  }
}
