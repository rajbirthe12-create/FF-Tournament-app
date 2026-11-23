# FF Tournament — Flutter Android project

এটি একটি Flutter/Firebase starter project। Player app এবং Admin panel একই অ্যাপে আছে।
এটি এখনই প্রস্তুত APK নয়; Firebase project যুক্ত করে build করতে হবে।

## যা আছে
- Email দিয়ে player registration/login
- Tournament list এবং Solo/Duo/Squad tournament publish
- bKash/Nagad payment method ও Transaction ID জমা
- Admin payment request approve/reject
- Approve করার সময় Room ID/Password প্রকাশ
- Winner name সংরক্ষণ

## প্রয়োজন
- Flutter SDK (stable)
- Android Studio / Android SDK
- Firebase account এবং একটি Firebase project

## চালু করার ধাপ
1. Flutter ইনস্টল করুন।
2. ZIP extract করে project folder-এ ঢুকে Android wrapper ও platform files তৈরি করতে `flutter create . --platforms=android` চালান।
3. Firebase Console-এ Android app তৈরি করুন। Package name: `com.example.ff_tournament`
4. Firebase Authentication থেকে Email/Password enable করুন।
5. Cloud Firestore database তৈরি করুন।
6. কম্পিউটারে FlutterFire CLI ইনস্টল করে project folder-এ চালান:
   `dart pub global activate flutterfire_cli`
   `flutterfire configure`
   এটি `lib/firebase_options.dart` তৈরি করবে এবং `main.dart`-এ `Firebase.initializeApp()`-কে নিচের মতো বদলান:
   `await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);`
   এবং `import 'firebase_options.dart';` যোগ করুন।
7. Firebase Console → Firestore → Rules-এ `firestore.rules`-এর নিয়ম publish করুন।
8. প্রথম admin বানাতে Authentication-এ নিজের account তৈরি করুন। Authentication → Users থেকে UID কপি করুন। Firestore-এ `admins` collection তৈরি করে document ID হিসেবে সেই UID দিন। Document-এর ভিতরে `role: "admin"` দিতে পারেন। এই admin document শুধু Console থেকেই তৈরি করা যাবে।
9. Project folder-এ:
   `flutter pub get`
   `flutter run`
10. APK বানাতে:
   `flutter build apk --release`
   APK পাওয়া যাবে: `build/app/outputs/flutter-apk/app-release.apk`

## bKash/Nagad payment setup
এই starter-এ কোনো টাকা স্বয়ংক্রিয়ভাবে কাটা হয় না। Player নিজে অনুমোদিত নম্বরে টাকা পাঠিয়ে Transaction ID জমা দেয়; Admin হাতে যাচাই করে approve করবে। অফিসিয়াল Merchant API/Payment Gateway যোগ করতে হলে বৈধ merchant credentials ও server-side backend লাগবে। Secret/API key কখনো Android app-এ রাখবেন না।

## গুরুত্বপূর্ণ
- Firestore-এ `settings/payment` document তৈরি করে `bkashNumber`, `nagadNumber` রাখার জন্য কাঠামো প্রস্তুত, তবে নম্বর দেখানোর UI এখনো যুক্ত করা হয়নি।
- Room ID/password শুধুমাত্র approved join record-এ app-এর My Matches-এ দেখানোর UI-তে পরবর্তী ধাপে যোগ করা যেতে পারে; বর্তমান সংস্করণে status এবং room field-এর সংরক্ষণ আছে।
- Winner save বাটন সর্বশেষ tournament-এ winner লিখে; পূর্ণাঙ্গ production ব্যবহারের আগে tournament picker, input validation, audit log, abuse protection এবং server-side payment verification যোগ করুন।
- Free Fire/Garena-এর সঙ্গে এই অ্যাপের কোনো আনুষ্ঠানিক সম্পর্ক নেই। তাদের trademark/logo ব্যবহারে অনুমতি নিশ্চিত করুন।
