# Membership Card

Demo: https://membership-card.pages.dev/

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

- Some browsers (Desktop Safari, iOS, Firefox) may not see the rotating gradient animation of the card's background. This is due to the lack of browser support for the @property CSS (https://developer.mozilla.org/en-US/docs/Web/CSS/@property). For such devices, you will see a static gradient.
