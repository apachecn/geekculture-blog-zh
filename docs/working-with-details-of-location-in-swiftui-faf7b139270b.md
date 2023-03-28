# 在 Swift 中处理位置细节

> 原文：<https://medium.com/geekculture/working-with-details-of-location-in-swiftui-faf7b139270b?source=collection_archive---------3----------------------->

![](img/de1daff4a75e1c58bda28a80f2426b60.png)

Photo by [Clay Banks](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将获取基于坐标的位置细节，反之亦然。这两段代码在使用 MapKit 时非常有用。

## 基于坐标提取细节

CLLocationManager 用于获取用户的坐标。我们将使我们的视图模型委托给 CLLocationManager。然后 CLLocationManager 将使用' didUpdateLocations '为我们提供坐标。

一旦我们获得用户的坐标，我们将使用 CLGeocoder 的 reverseGeocodeLocation 函数来获得 CLPlacemark。从这个地标，我们将解码位置细节如下:

```
func update() {let latitude = location?.latitude ?? 0let longitude = location?.longitude ?? 0self.region = MKCoordinateRegion(center:CLLocationCoordinate2D(latitude:                                                                       CLLocationDegrees(latitude), longitude: CLLocationDegrees(longitude)), span:                                 MKCoordinateSpan(latitudeDelta: 0.5, longitudeDelta: 0.5))CLGeocoder().reverseGeocodeLocation(CLLocation(latitude: latitude, longitude: longitude)) { placemarks, error inguard let placemark = placemarks?.first else {return}let reversedGeoLocation = GeoLocation(with: placemark)
self.name = reversedGeoLocation.name

  }

}
```

地理位置是我们用来解码地标细节的自定义模型。

```
struct GeoLocation {let name: Stringlet streetName: Stringlet city: Stringlet state: Stringlet zipCode: Stringlet country: Stringinit(with placemark: CLPlacemark) {self.name           = placemark.name ?? ""self.streetName     = placemark.thoroughfare ?? ""self.city           = placemark.locality ?? ""self.state          = placemark.administrativeArea ?? ""self.zipCode        = placemark.postalCode ?? ""self.country        = placemark.country ?? ""}}
```

通过这种方式，我们可以使用坐标获取位置的详细信息。

## 根据地址获取坐标

当用户以任何方式输入地址时，我们可以使用 CLGeocoder 对地址字符串进行如下地理编码:

```
func reverseUpdate() {let geocoder = CLGeocoder()geocoder.geocodeAddressString(name) { placemarks, error in

guard error == nil else { return}

guard let placemark = placemarks?[0] else {return}let coord = placemark.location?.coordinate ?? CLLocationCoordinate2D(latitude:
CLLocationDegrees(0), longitude: CLLocationDegrees(0)) self.region = MKCoordinateRegion(center: coord, span:
MKCoordinateSpan(latitudeDelta: 0.5, longitudeDelta: 0.5))self.location = CLLocationCoordinate2D(latitude: placemark.location?.coordinate.latitude ?? 0, longitude: placemark.location?.coordinate.longitude ?? 0)

}
```

CLGeocoder 将给出 CLPlacemark，我们将通过它获得位置的坐标。

**完整视图模型:**

以下代码可以与来自 [post](/@maheshsai252/search-using-mapkit-in-swiftui-636426836ab) 的代码一起使用:

你可能也会觉得这篇文章很有用:

[https://medium . com/@ maheshsai 252/search-using-map kit-in-swift ui-636426836 ab](/@maheshsai252/search-using-mapkit-in-swiftui-636426836ab)