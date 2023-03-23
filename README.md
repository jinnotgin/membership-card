# Membership Card
Demo: https://membership-card.pages.dev/

https://user-images.githubusercontent.com/14935308/227180947-30d18f90-538c-4145-9e08-4f35dd34e3e8.mp4

## Modifiable URL Parameters

- `name`
- `slogan`
- `footer1`
- `footer2`
- `logo`
- `color`
- `data` (JSON data that will be stored in the QR Code)

> Sample URL: `https://membership-card.pages.dev/?name=Nicole&slogan=Restaurateur&footer1=29/12/23&footer2=12:00AM&logo=https://i.imgur.com/w0U4OVv.jpg&color=dd1c1f&data={"id":nicole123}`


## Known Issues
- For iOS 14+ devices, the card will *not* tilt based on device's motion. This is becasue iOS 14+ will require explict user permission approval to access Device Motion data. In our user testing, we have found that users are not familiar with the "Device Motion" permission prompt, and was found the prompt to be undesirable.
- Some browsers (Desktop Safari, iOS, Firefox) may not see the rotating gradient animation of the card's background. This is due to the lack of browser support for the @property CSS (https://developer.mozilla.org/en-US/docs/Web/CSS/@property). For such devices, you will see a static gradient.
