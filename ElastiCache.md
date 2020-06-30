### Question:

1. Is it possible to stop nodes in AWS ElastiCache cluster

    ElastiCache clusters cannot be stopped. They can only be deleted and recreated. You can use this pattern to avoid paying for time when you're not using the cluster.

    If you are using a Redis ElastiCache cluster, you can create a snapshot as the cluster is being deleted. Then, you can restore the cluster from the snapshot when you create it. This way, you preserve the data in the cluster.

    The cluster endpoints are derived from a combination of

    - the cluster IDs,
    - the region,
    - the AWS account.

    So as long as you delete and re-create clusters with those parts being constant, then the clusters will maintain the same endpoint.