# SUMO Smart Contracts Errors

This document lists all errors thrown by **SubscriptionManagerFactory** and **SubscriptionManager** contracts, along with their meaning.  

Use this as a reference for API integration and frontend error handling.

## SubscriptionManagerFactory Errors

| Error | Meaning |
|-------|---------|
| `InvalidAddress` | The provided address is 0 or otherwise invalid. |
| `InvalidSubscriptions` | Maximum subscriptions value is 0 or exceeds allowed limits. |

## SubscriptionManager Errors

| Error | Meaning |
|-------|---------|
| `NotOwner` | Caller is not the owner of the subscription. |
| `InvalidAddress` | Provided address is 0 or otherwise invalid. |
| `InvalidSubscriptions` | Subscription creation parameters are invalid or exceed allowed limits. |
| `NotSubscribed` | The address is not an active subscription owner. |
| `SubscriptionExpired` | The subscription has expired. |
| `MaxAccountsReached` | Maximum number of secondary accounts reached. |
| `MaxModificationsReached` | Maximum number of allowed revocations reached. |
| `AlreadyAuthorized` | Account is already linked as secondary. |
| `MaxSubscriptionsReached` | Maximum subscriptions per manager reached. |
| `AlreadySubscribed` | Owner already has a subscription. |
| `InvalidDuration` | Duration provided is 0 or invalid. |
| `OwnerCannotBeSecondary` | Owner account cannot be added as secondary. |
| `InvalidPlanId` | Provided planId is invalid. It must be greater than 0 and different from the current planId. |

