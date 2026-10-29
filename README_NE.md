# JK Broadcaster Android Project (Starter)

यो Android Studio/Gradle project ले:
- JK Broadcaster नामको Android app shell बनाउँछ।
- Server host/port/mount/user/password र जिंगल URI राख्ने UI दिन्छ।
- Foreground service सुरु/बन्द गर्छ।
- स्थानीय समयअनुसार हरेक घण्टाको :00 मा, 06:00 देखि 22:00 सम्म जिंगल schedule trigger गर्छ।

## महत्त्वपूर्ण सीमा
यो **अहिले पूर्ण LIVE broadcasting APK होइन**। PCM audio mixer, microphone capture, media decoding, encoder र Icecast/Shoutcast source protocol पूरा जोडिएको छैन। Service को TODO टिप्पणीमा आवश्यक implementation points छन्। जिंगल बज्ने बेला स्थानीय फोनमा मात्रै बजाउनु पर्याप्त हुँदैन; जिंगललाई encoder अघि broadcast PCM stream मै mix गर्नुपर्छ।

## Build
1. Android Studio मा project folder खोल्नुहोस्।
2. Gradle sync पूरा हुन दिनुहोस् (JDK 17 सिफारिस)।
3. Build > Build Bundle(s) / APK(s) > Build APK(s) छान्नुहोस्।
4. APK सामान्यतया `app/build/outputs/apk/debug/app-debug.apk` मा आउँछ।

मोबाइलमै build गर्न GitHub मा repository राखेर GitHub Actions Android build workflow थप्न सकिन्छ; GitHub Actions workflow तयार गरी चलाउनु पर्ने हुन्छ।

## Schedule defaults
- Hourly at minute 00
- Active window: 06:00–22:00 (inclusive)
- Jingle gain target: 80%
- Music ducking: target music gain reduced while jingle plays (not yet wired to PCM mixer)

## Production completion checklist
- [ ] Storage Access Framework file picker for audio files
- [ ] AudioRecord mic capture with foreground service
- [ ] MediaExtractor/MediaCodec PCM decoding for music and jingles
- [ ] PCM mixer with 80% jingle gain and music ducking
- [ ] MediaCodec encoder and buffering
- [ ] Icecast SOURCE/PUT authentication and metadata handling
- [ ] Shoutcast v1/v2 source protocol support
- [ ] Reconnect/backoff, network errors, lifecycle, audio focus
- [ ] Test on target Android phones and actual radio server

## GitHub Actions
`.github/workflows/android-apk.yml` मा APK build workflow छ। Android project मा Gradle Wrapper (`gradlew`, `gradle/wrapper/`) नसमेटिएको हुनाले GitHub runner मा `gradle` command उपलब्ध हुनुपर्छ; यदि build fail भयो भने Android Studio बाट Gradle Wrapper generate गरेर repository मा commit गर्नुहोस्। Workflow चलाउन GitHub repository को Actions tab मा गएर `Build JK Broadcaster APK` run गर्नुहोस्।
