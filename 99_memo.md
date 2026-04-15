```
// DomainEvent
export interface DomainEvent {
  readonly type: string;
  readonly occurredAt: string;
  readonly payload?: unknown;
}

// Domain側が依存するポート
export interface DomainEventDispatcher {
  publish(event: DomainEvent): Promise<void>; // 非同期を想定
}
```

```
import { Subject } from 'rxjs';
import { DomainEventDispatcher, DomainEvent } from '../../domain/events';

export class RxEventBus implements DomainEventDispatcher {
  private subject = new Subject<DomainEvent>();
  publish(e: DomainEvent): Promise<void> {
    this.subject.next(e);
    return Promise.resolve();
  }
  // projections 用に購読インターフェースを公開
  subscribe(handler: (e: DomainEvent) => void) {
    return this.subject.subscribe(handler);
  }
}
```

```
class MatchAggregate {
  constructor(private readonly events: DomainEventDispatcher) {}

  scorePoint() {
    // ドメインの状態変更...
    this.events.publish({
      type: 'MatchScoreChanged',
      occurredAt: new Date().toISOString(),
      payload: { matchId: 'm1', score: { a: 10, b: 8 } },
    });
  }
}
```

```
// projections 側で RxEventBus を受け取り購読
eventBus.subscribe(ev => {
  if (ev.type === 'MatchScoreChanged') {
    // PointHistoryStore (例: `point-history.store.ts`) の subject を更新
    pointHistoryStore.replaceAll(/* map from ev.payload */);
  }
});
```

```
// イベントマップ
type DomainEventMap = {
  MatchScoreChanged: { matchId: string; score: { a: number; b: number } };
  PointAdded: { matchId: string; pointId: string; winner: 'A' | 'B' };
};

// 汎用 DomainEvent
type DomainEvent<K extends keyof DomainEventMap = keyof DomainEventMap> = {
  type: K;
  occurredAt: string;
  payload: DomainEventMap[K];
};

// ジェネリック EventBus
class TypedRxEventBus {
  private subject = new Subject<DomainEvent>();

  publish<E extends keyof DomainEventMap>(type: E, payload: DomainEventMap[E]) {
    this.subject.next({ type, occurredAt: new Date().toISOString(), payload });
  }

  ofType<E extends keyof DomainEventMap>(type: E) {
    return this.subject.asObservable().pipe(
      filter((e): e is DomainEvent<E> => e.type === type)
    );
  }
}

eventBus.ofType('MatchScoreChanged').subscribe(ev => {
  // ev.payload は { matchId: string; score: { a:number; b:number } } として型安全
});

```