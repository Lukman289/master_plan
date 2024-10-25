# Nama  : Lukman Eka Septiawan
# Kelas : TI-3C

## Praktikum 1: Dasar State dengan Model-View
### Lankah 1: Buat Project Baru
```dart

```
### Lankah 2: Membuat model task,dart
```dart
class Task {
  final String description;
  final bool complete;
  
  const Task({
    this.complete = false,
    this.description = '',
  });
}
```
### Lankah 3: Buat file plan.dart
```dart
import './task.dart';

class Plan {
  final String name;
  final List<Task> tasks;
  
  const Plan({this.name = '', this.tasks = const []});
}
```
### Lankah 4: Buat file data_layer.dart
```dart
export 'plan.dart';
export 'task.dart';
```
### Lankah 5: Pindah ke file main.dart
```dart
import 'package:flutter/material.dart';
import './views/plan_screen.dart';

void main() => runApp(MasterPlanApp());

class MasterPlanApp extends StatelessWidget {
  const MasterPlanApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
     theme: ThemeData(primarySwatch: Colors.purple),
     home: PlanScreen(),
    );
  }
}
```
### Lankah 6: Buat plan_screen.dart
```dart
import '../models/data_layer.dart';
import 'package:flutter/material.dart';

class PlanScreen extends StatefulWidget {
  const PlanScreen({super.key});

  @override
  State createState() => _PlanScreenState();
}

class _PlanScreenState extends State<PlanScreen> {
  Plan plan = const Plan();

  @override
  Widget build(BuildContext context) {
   return Scaffold(
    // ganti ‘Namaku' dengan Nama panggilan Anda
    appBar: AppBar(title: const Text('Master Plan Namaku')),
    body: _buildList(),
    floatingActionButton: _buildAddTaskButton(),
   );
  }
}
```
### Lankah 7: Buat method _buildAddTaskButton()
```dart
Widget _buildAddTaskButton() {
  return FloatingActionButton(
   child: const Icon(Icons.add),
   onPressed: () {
     setState(() {
      plan = Plan(
       name: plan.name,
       tasks: List<Task>.from(plan.tasks)
       ..add(const Task()),
     );
    });
   },
  );
}
```
### Lankah 8: Buat widget _buildList()
```dart
Widget _buildList() {
  return ListView.builder(
   itemCount: plan.tasks.length,
   itemBuilder: (context, index) =>
   _buildTaskTile(plan.tasks[index], index),
  );
}
```
### Lankah 9: Buat widget _buildTaskTile
```dart
 Widget _buildTaskTile(Task task, int index) {
    return ListTile(
      leading: Checkbox(
          value: task.complete,
          onChanged: (selected) {
            setState(() {
              plan = Plan(
                name: plan.name,
                tasks: List<Task>.from(plan.tasks)
                  ..[index] = Task(
                    description: task.description,
                    complete: selected ?? false,
                  ),
              );
            });
          }),
      title: TextFormField(
        initialValue: task.description,
        onChanged: (text) {
          setState(() {
            plan = Plan(
              name: plan.name,
              tasks: List<Task>.from(plan.tasks)
                ..[index] = Task(
                  description: text,
                  complete: task.complete,
                ),
            );
          });
        },
      ),
    );
  }
```
### Lankah 10: Tambah Scroll Controller
```dart
class _PlanScreenState extends State<PlanScreen> {
  Plan plan = const Plan();
  late ScrollController scrollController;
```
### Lankah 11: Tambah Scroll Listener
```dart
@override
void initState() {
  super.initState();
  scrollController = ScrollController()
    ..addListener(() {
      FocusScope.of(context).requestFocus(FocusNode());
    });
}
```
### Lankah 12: Tambah controller dan keyboard behavior
```dart
return ListView.builder(
  controller: scrollController,
 keyboardDismissBehavior: Theme.of(context).platform ==
 TargetPlatform.iOS
    ? ScrollViewKeyboardDismissBehavior.onDrag
    : ScrollViewKeyboardDismissBehavior.manual,
```
### Lankah 13: Terakhir, tambah method dispose()
```dart
@override
void dispose() {
  scrollController.dispose();
  super.dispose();
}
```
### Lankah 14: Hasil
![photo result practicum 1](lib\assets\images\result-p1.jpg)
![gif result practicum 1](lib\assets\gifs\gif-p1.gif)

### Tugas Praktikum 1
1. Selesaikan langkah-langkah praktikum tersebut, lalu dokumentasikan berupa GIF hasil akhir praktikum beserta penjelasannya di file README.md! Jika Anda menemukan ada yang error atau tidak berjalan dengan baik, silakan diperbaiki.

2. Jelaskan maksud dari langkah 4 pada praktikum tersebut! Mengapa dilakukan demikian?
> file plan.dart dan task.dart diekspor melalui data_layer.dart. Ketika file data_layer.dart diimpor di tempat lain, file plan.dart dan task.dart otomatis tersedia juga. kode tersebut digunakan untuk mencegah kode menjadi berantakan dengan impor dari berbagai file yang tersebar.

3. Mengapa perlu variabel plan di langkah 6 pada praktikum tersebut? Mengapa dibuat konstanta ?
> Variabel plan digunakan untuk menyimpan data dari objek Plan. Objek ini mungkin berisi informasi penting seperti nama rencana (plan) dan tanggal jatuh tempo (dueDate). Jadi, variabel ini diperlukan untuk menyimpan dan mengelola informasi yang relevan dengan tugas dalam aplikasi tersebut. Variabel tersebut dibuat konstanta (const) berarti bahwa objek Plan yang diwakili oleh variabel plan tidak akan pernah berubah setelah diinisialisasi.

4. Lakukan capture hasil dari Langkah 9 berupa GIF, kemudian jelaskan apa yang telah Anda buat!
![gif result practicum 1](lib\assets\gifs\gif-p1.gif)
> Kode tersebut adalah sebuah metode bernama _buildTaskTile yang mengembalikan sebuah widget ListTile, digunakan untuk menampilkan tugas (Task) dengan kontrol checkbox dan TextFormField di dalam daftar (list). Kode ini mengimplementasikan pembaruan state dari objek plan ketika pengguna mengubah status Task (melalui checkbox) atau deskripsinya (melalui TextFormField).

5. Apa kegunaan method pada Langkah 11 dan 13 dalam lifecyle state ?
> method pada langkah 11 berfungsi untuk menginisialisasi widget Stateful saat pertama kali dibangun. Di dalamnya, sebuah ScrollController diinisialisasi untuk memantau pergerakan scroll pada widget yang mendukung scrolling. Sebuah listener ditambahkan ke ScrollController, yang secara otomatis mengalihkan fokus dari elemen UI yang sedang aktif (seperti input field) setiap kali pengguna menggulir. sedangkan untuk langkah 13 berfungsi untuk membersihkan resource yang terkait dengan widget, seperti ScrollController. method tersebut juga dapat membantu memaksimalkan penggunaan resource aplikasi seperti menghapus widget yang sudah tidak digunakan.

6. Kumpulkan laporan praktikum Anda berupa link commit atau repository GitHub ke spreadsheet yang telah disediakan!