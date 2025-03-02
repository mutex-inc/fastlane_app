# CDのexample app
### (1) 結論
- fastlaneは使用せずに、TestFlightの配布を行いました。

### (2) 必要な環境変数
- `APP_STORE_CONNECT_API_KEY`
- `ISSURE_ID`
- `KEY_ID`

[こちらの記事が画像付きで分かりやすいです](https://arc.net/l/quote/pheghqeb)
![image](https://github.com/user-attachments/assets/3fba0216-61be-4555-92ff-9045b9edf7ee)


### (3) `ExportOptions.plist(destination: upload)` + `xcodebuild -exportArchive`でTestFlightの配布を行う
- `ExportOptions.plist` - `destination`: `upload` ([ref](https://qiita.com/taisuke_h/items/e37d96c96a811b630c0c#destination))
- `xcodebuild`: `-exportArchive`
```
xcodebuild -exportArchive // specifies that an archive should be exported
           -archivePath build/ios/Runner.xcarchive // specifies the directory where any created archives will be placed, or the archive that should be exported
           -exportOptionsPlist ${{ env.export_options_plist }} // specifies a path to a plist file that configures archive exporting
           -exportPath build/ios/ipa // specifies the destination for the product exported from an archive
           -allowProvisioningUpdates // Allow xcodebuild to communicate with the Apple Developer website. For automatically signed targets, xcodebuild will create and update profiles, app IDs, and certificates. For manually signed targets, xcodebuild will download missing or updated provisioning profiles. Requires a developer account to have been added in Xcode's Accounts settings or an App Store Connect authentication key to be specified via the -authenticationKeyPath, -authenticationKeyID, and -authenticationKeyIssuerID parameters.
           -authenticationKeyIssuerID ${{ secrets.ISSUER_ID }} 
           -authenticationKeyID ${{ secrets.KEY_ID }} 
           -authenticationKeyPath `pwd`/private_keys/AuthKey_${{ secrets.KEY_ID }}.p8

各コメントは xcodebuild -help から
```

