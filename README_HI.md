# Sohid Ludo Bluetooth — Android Studio project

इस प्रोजेक्ट में दो अलग Android apps हैं:
- `game` → Ludo Game APK (Bluetooth server)
- `controller` → Controller APK (Bluetooth client)

## जरूरी बातें
- दोनों फोन Android पर हों और Bluetooth चालू हो।
- Android Settings में दोनों फोन को पहले pair करें।
- यह starter project अपने गेम को कंट्रोल करता है; यह किसी तीसरे पक्ष के Ludo app को नियंत्रित/हैक नहीं करता।
- Bluetooth Classic RFCOMM का उपयोग है। फोन/Android निर्माता के अनुसार pairing और permission prompts अलग दिख सकते हैं।
- यह playable starter है, किसी commercial Ludo app की exact pixel-for-pixel copy नहीं। बोर्ड की बनावट भेजी गई तस्वीर से प्रेरित है।

## APK बनाने का तरीका
1. Android Studio में इस folder को Open करें।
2. Gradle sync पूरा होने दें (JDK 17 इस्तेमाल करें)।
3. `game` configuration/module चुनकर Run करें या Build > Build APK(s) करें।
4. `controller` module के लिए भी यही करें।
5. APK आम तौर पर `game/build/outputs/apk/debug/game-debug.apk` और
   `controller/build/outputs/apk/debug/controller-debug.apk` में मिलेंगे।

## इस्तेमाल
1. दोनों APK अलग-अलग Android फोन में install करें।
2. फोन की Bluetooth settings में दोनों डिवाइस pair करें।
3. Game फोन पर Sohid Ludo Game खोलें और Bluetooth permission दें। स्क्रीन पर "Waiting for controller" दिखेगा।
4. Controller फोन पर Sohid Ludo Controller खोलें, paired devices में Game फोन चुनें और Connect दबाएँ।
5. कनेक्ट होने के बाद Roll Dice और Move Token 1–4 बटन इस्तेमाल करें।

## कंट्रोल प्रोटोकॉल
Controller एक-एक newline-terminated text command भेजता है:
- `ROLL`
- `MOVE:0`, `MOVE:1`, `MOVE:2`, `MOVE:3`

Game app commands को main UI thread पर process करता है। Game app हर समय वैध चाल की जाँच करता है; Controller केवल अनुरोध भेजता है।

## Bluetooth permissions
Android 12+ पर `BLUETOOTH_CONNECT` runtime permission मांगी जाती है। Android 11 और पुराने पर legacy Bluetooth permissions manifest में हैं।
