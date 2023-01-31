# Vault Interface

## Partition

一个 __Vault__ 可以有多个 __Partition__。每个 __Partition__ 应该有一个特定的用途，不同用途的 __Partition__ 不应该混合使用。例如，一个给A共享数据的 __Partition__ 不要为专门 __Byte Factor__ 创建的 __Partition__ 混合使用。__Partition__ 不再需要后，可以清除。

```go title=''Partition Interface'
type Partition interface {
 // CreatePartition creates a new Partition, return Partition ID, nil if seccuss otherwise return nil and an error
 // purpose the other part vault id or data factory id
 CreatePartition(purpose []byte) ([]byte, error)

 // CleanPartition cleans data for Partition
 CleanPartition(Partition []byte) error

 // lockPartition locks the Partition, which causes only controllers can access the Partition, others cannot
 LockPartition(Partition []byte)

 // GrantRead allows vaultid read data from Partition
 GrantRead(Partition []byte, vaultID []byte) error
 RevokeRead(Partition []byte, vaultid []byte) error

 // GrantWrite allows vaultID write data to Partition
 GrantWrite(Partition []byte, vaultID []byte) error
 RevokeWrite(Partition []byte, vaultID []byte) error

 // GrantUpdate grant vaultID update existing data in Partition
 GrantUpdate(Partition []byte, vaultid []byte) error
 RevokeUpdate(Partition []byte, vaultid []byte) error

// Read reads key's value in Partition partiion
 Read(Partition []byte, key []byte) ([]byte, error)
 
 // Write write value for key in Partition partiion
 Write(Partition []byte, key []byte, value []byte) error

 // Update updates value for key in Partition partiion
 Update(Partition []byte, key []byte, value []byte) error
}
```

## Meta Partition

__Meta Partition__ 是特殊分区，并且是不可以删除的，也不可以与其他人共享，仅有该分区的持有人可以访问该分区数据。该分区存储Vault的元数据。

## Primary Partition

__Primary Partition__ 是另一个特殊分区，并且是不可以删除的，也不可以与其他人共享，仅有该分区的持有人可以访问该分区数据。该分区存储持有人的用户私有数据。

## Collabrative Partition

__Collabrative Partition__ 是普通共享分区，该分区的数据用于共享。 分享类型包括 __Read__、__Write__ 以及 __Update__，并且3种操作相互独立，有写权限的用户，可以没有读权限。

## Operating

- __Read__
- __Write__
- __Update__

按照 __Key-Value__ 形式读、写和更新数据。
