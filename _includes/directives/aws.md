It is sometimes very tedious and hard to generate AWS signatures. 
So we have provided a helper directive, that will simplify this task. 
Below are all the properties that you can use to customize the signature generation.

| Key              | Type                                        | Description                                                                                                                                     |
| ---              | ---                                         | ---                                                                                                                                             |
| **key**          | [IML String](articles/types.md#iml-string)  | AWS key                                                                                                    |
| **secret**       | [IML String](articles/types.md#iml-string)  | AWS secret                                                                                                 |
| **session**      | [IML String](articles/types.md#iml-string)  | AWS session. Note that this only works for services that require session as part of the canonical string   |
| **bucket**       | [IML String](articles/types.md#iml-string)  | AWS bucket, unless you’re specifying your bucket as part of the path, or the request doesn’t use a bucket  |
| **sign_version** | [IML String](articles/types.md#iml-string)  | **Default:** 2. AWS sign version. Must be either 2 or 4.                                                   |