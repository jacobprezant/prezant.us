---
date:
    created: 2025-07-30
---

# Reversing macOS 26's `.icon`

<!-- more -->

With the release of MacOS 26 (Tahoe), we now have an additional application bundled with Xcode 26; Icon Composer. Icon Composer lets developers create "layered icons out of Liquid Glass from a single design" in
a new multi-layer format. This introduces the `.icon` extension, the standard format from Icon Composer. These files can then be imported into Xcode 26, and specified in the `App Icons and Launch Screen` settings for your project.

With this new proprietary format, we'll need to understand how the icons are stored and bundled to implement support for `.icon` files in our applications.

The `.icon` file itself is simply a folder, in which there is an `icon.json` file and an `assets` subfolder. This JSON file is what Icon Composer interprets. It's made up of key-value pairs pointing to files within `assets` within a `layers` object along with other icon details like `translucency`, an sRGB for the gradient, `shadow`, etc.

See a sample `icon.json` for a simple icon made with Icon Composer:
``` json title="icon.json"
{
  "fill" : {
    "automatic-gradient" : "extended-srgb:0.00000,0.47843,1.00000,1.00000"
  },
  "groups" : [
    {
      "layers" : [
        {
          "image-name" : "mobile-solid.svg",
          "name" : "mobile-solid"
        }
      ],
      "shadow" : {
        "kind" : "neutral",
        "opacity" : 0.5
      },
      "translucency" : {
        "enabled" : true,
        "value" : 0.5
      }
    }
  ],
  "supported-platforms" : {
    "circles" : [
      "watchOS"
    ],
    "squares" : "shared"
  }
}
```

As a takeaway, the `.icon` format seems simple to reproduce. In regards to implementation, we still see `.png` files within the `.app`, however the `assets.car` is signifigantly modified when working with `.icon` rather than the current `.xcassets`.
