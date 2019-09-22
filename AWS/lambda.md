## ブログ書いた

- https://www.tomcky.net/entry/20190914/1568444453

## トラブルシュート

```
The serverless deployment bucket "xxxxx" does not exist. Create it manually if you want to reuse the CloudFormation stack "xxxxx", or delete the stack if it is no longer required.
```

CloudFormationを開いて対象のスタックを削除するか、S3にバケットを作るか、で解決できる。

## S3へのファイルアップロードをトリガーにする

https://qiita.com/Futo23/items/fc2f9e472e6a2765d15d

```ts
console.log("Let's Start!!!!!");

export const hello = async (event, _context) => {
    var Bucket = event.Records[0].s3.bucket.name;
    var ObjKey = event.Records[0].s3.object.key;
    var FilePath = Bucket + '/' + ObjKey;
    console.log('BucketName : ' + Bucket);
    console.log('ObjKey : ' + ObjKey);
    console.log('FilePath : ' + FilePath);
};
```

async/await積極的に使っていきましょう。