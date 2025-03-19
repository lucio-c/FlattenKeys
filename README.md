```
 type Number9 = [never, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
    /** 深层对象的 key */
    /** 检查是否为对象类型 */
    type IsObject<T> = T extends object ? (T extends Array<unknown> ? false : true) : false;

    /** 防止循环引用的类型映射 */
    type Visited<T> = {
        [K in keyof T]: true;
    };

    /** 改进后的 FlattenKeys 类型，增加循环引用检测 */
    type FlattenKeys<
        T = object,
        Prefix extends string = "",
        Depth extends Number9[number] = 9,
        Seen = {},
    > = T extends object
        ? Seen extends Visited<T>
            ? never
            : {
                  [K in keyof T]: Depth extends never
                      ? `${Prefix}${K & string}`
                      : IsObject<T[K]> extends true
                        ?
                              | `${Prefix}${K & string}`
                              | FlattenKeys<
                                    Exclude<T[K], null | undefined>,
                                    `${Prefix}${K & string}.`,
                                    Number9[Depth],
                                    Seen & Visited<T>
                                >
                        : `${Prefix}${K & string}`;
              }[keyof T]
        : never;
```
