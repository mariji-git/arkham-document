```text
src/app/
  ├── membership/          # 会員管理コンテキスト（入会、契約、支払）
  │    ├── projections/    # Projections（イベントを検知してSignalを更新）
  │    ├── read-models/    # Read Models（Signalで扱う表示専用のデータ型）
  │    └── components/     # Angular Components（画面、テンプレート）
  ├── access-control/      # 入退館コンテキスト（自動改札、ゲート制御、滞在管理）
  │    ├── projections/    # Projections（イベントを検知してSignalを更新）
  │    ├── read-models/    # Read Models（Signalで扱う表示専用のデータ型）
  │    └── components/     # Angular Components（画面、テンプレート）

       
src/libs/
  ├── membership//
  │       ├── domain/              # 【ドメイン層】業務ルールと事実の定義
  │       │    ├── models/         # Entity, Value Object（会員、ゲート）
  │       │    ├── services/       # Domain Services（入館許可判定ロジック）
  │       │    └── events/         # Domain Events（MemberEntered, MemberExited） # 事実の定義（例：入会した、入館・退館したなど）
  │       │
  │       ├── use-cases/           # 【アプリケーション層】連鎖の調整
  │       │    ├── entry-flow/     # 入館の一連の処理（ユースケース単位で作成する）
  │       │    └── saga/           # Saga（入館に伴う複数ドメインにまたがる連鎖制御）
  │       │
  │       ├── infrastructure/      # 【インフラ層】外部・技術への依存
  │       │    ├── repositories/   # DB保存（Event Store）の実装
  │       │    └── gate-api/       # ACL（実際の改札機センサーとの通信変換）
  ├── access-control/
  │       ├── domain/              # 【ドメイン層】業務ルールと事実の定義
  │       │    ├── models/         # Entity, Value Object（会員、ゲート）
  │       │    ├── services/       # Domain Services（入館許可判定ロジック）
  │       │    └── events/         # Domain Events（MemberEntered, MemberExited） # 事実の定義（例：入会した、入館・退館したなど）
  │       │
  │       ├── use-cases/           # 【アプリケーション層】連鎖の調整
  │       │    ├── entry-flow/     # 入館の一連の処理（ユースケース単位で作成する）
  │       │    └── saga/           # Saga（入館に伴う複数ドメインにまたがる連鎖制御）
  │       │
  │       ├── infrastructure/      # 【インフラ層】外部・技術への依存
  │       │    ├── repositories/   # DB保存（Event Store）の実装
  │       │    └── gate-api/       # ACL（実際の改札機センサーとの通信変換）
```
