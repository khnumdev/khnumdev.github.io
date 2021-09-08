---
layout: post
title:  "Development environment with cloud integration"
date:   2021-09-10 16:24:20 +0200
categories: engineering
---

Years ago it was not as usual, but now most of our developments have a direct integration -and dependencies- with cloud platforms such as Azure, GCP or AWS. Let's speak here about different strategies about how can live with that since the beginning. Most of these services are going to be Paas or SaaS, so probably there will be some libraries/packages that can be used for their usage.

But before starting with this, let's stablish the initial mandatory idea: our environment needs to be as *closest as possible* as production environment. With that idea in mind, we are going to discuss several alternatives for having 

## Alternatives

As an example, we are going to use two kind of services: Storage and some computing for big data. To simplify it, let's see that we are going to use *Azure Storage* and *Azure Databricks*. For both services there are *NuGet* packages where all the libraries required for using and managing the resources are available.

We could just use the ones provided directly, but it is a good pattern to encapusaled them in our own service:

```cs
public class StorageService 
{

    public StorageService(Credentials credentials) 
    {

    }

    public IList<Blob> GetBlobs(String container, String path) 
    {
      ...
    }

    public void Upload(MemoryStream stream, String container, String path)
    {
      ...
    }

    // etc
}
``` 

```cs
public class DatabricksService 
{

 public DatabricksService(Credentials credentials) 
 {

}

  public QueryResult executeQuery(String query) 
  {

  }
}
```

Those are an example. I`ve put the credentials as part of the constructor but of course they can be as part of the methods or using a factory. This is not important here.

Normally with this pattern we could say *we have the app prepared to use different cloud providers for the future*. Ok, it is a good idea but due my experience this is not going to happen in the 99% of the times -I left 1% of error-. Finding the middle point between isolation and "for the future" is hard, but the only reason to have our own services are:
- Use our own model in the way that we want to use it.
- Use only the methods that are required by our model.

So, can be this helpful if we decide to support Azure Storage and GCP Cloud Storage at same time? Yeah. Is it the main purpose? Nope.

Having saying this, let's discuss some alternatives.

### Mock services

This is the typical thing that we are going to use for a unit test, but for development purposes. In our dependency system we could have something like:

```cs
if (IsDevelopment()) 
{
  // This is a custom service, sharing the same interface than RealStorageService but with methods that do nothing
  services.add<MockedStorageService>();
}
else
{
  services.add<RealStorageService>();
}
```



### Emulators

### Sharing a resource

### Environment per developer

## Summary