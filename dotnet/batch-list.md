#  Divide list into batches
In cases where  you want the handle a list of items in equally sized batches:

```csharp
private static IEnumerable<IEnumerable<T>> Batch<T>(this IEnumerable<T> collection, int batchSize)
{
    List<T> nextbatch = new List<T>(batchSize);
    foreach (T item in collection)
    {
        nextbatch.Add(item);
        if (nextbatch.Count == batchSize)
        {
            yield return nextbatch;
            nextbatch = new List<T>(batchSize);
        }
    }
    if (nextbatch.Count > 0)
        yield return nextbatch;
}

```