---
title: Getting Started
layout: default
---

# Getting Started

## Install

```bash
dotnet add package SmsBridge
```

## Register

```csharp
using SmsBridge.Abstractions;
using SmsBridge.DependencyInjection;
using SmsBridge.Options;

builder.Services.AddSmsBridge(opts =>
    {
        opts.DefaultProvider = "twilio";
        opts.Providers["twilio"] = new SmsProviderOptions { Type = SmsProviderType.Twilio };
    })
    .UseTwilio("twilio", o =>
    {
        o.AccountSid = builder.Configuration["SmsBridge:Providers:twilio:AccountSid"];
        o.AuthToken  = builder.Configuration["SmsBridge:Providers:twilio:AuthToken"];
        o.From       = builder.Configuration["SmsBridge:Providers:twilio:From"];
    });
```

## Send

```csharp
using SmsBridge.Abstractions;

public sealed class MyService(ISmsClient smsClient)
{
    public async Task NotifyAsync(string phone, string message) =>
        await smsClient.SendAsync(new SmsMessage { To = phone, Body = message });
}
```

Your application code depends only on `ISmsClient`. It never references a provider type.
