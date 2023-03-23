# Technical specifications for trainees

Win a chance to show your skills in iOS development in a cool company and become part of a large team of experts. Write your test project, and we will evaluate it and decide whether you’re ready to become a trainee in our iOS development team at Surf.

## What do you need to do?

You need to build a layout of a screen so that it fully replicates the [design](https://www.figma.com/file/S4ucVLUHYc0vLg2p1Xnart/IOS-%D1%81%D1%82%D0%B0%D0%B6%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0?node-id=45%3A77&t=N4eUtEGJu7LxSAnC-1).

Apart from the layout in UIKit, you are in no way limited in your choice of ways to do the task. Will it be MVP or VIPER? UITableView or ScrollView? You decide!

Presentation data should be wired in the app — no network requests.

The task has two versions: basic and advanced. The basic one describes the general requirements for the screen. The advanced one provides options for its improvement, easiest to hardest. Some options are mutually exclusive so you don’t need to do all of them (but the more hard ones you do, the higher your chances).

### Basic version:

- The image should follow the proportions in the first layout;
- The screen is static, not scrollable;
- The carousel should have 10 items tops and the carousel can be scrolled to the right. The width of an item in the carousel should depend on the text;
- Once the “Send request” button is pressed, the app should display a system alert with a header saying “Well done!” and the text “Your request has been successfully sent!” and a button that reads “Close”.

### Advanced version:

- All buttons have a press state. Color changes are your call;
- The screen is scrollable, images scroll together with the content;
- “Send request” and “Want to join us?” items should be nailed to the bottom of the screen and not scrollable;
- Once tapped, an item in the carousel should change its status to picked, and should return to its regular state when tapped again;
- Once tapped, an item in the carousel should transition into the first position with an animation;
- Looped carousel, i.e. it can be scrolled in any direction as much as you want. That said, there should still be 10 items in it with the tenth one followed by the first one;
- Extra carousel with items in it grouped in two rows and fitting in depending on the width of cells. The items that don’t fit into the two rows on the screen should not be displayed;
- Only the content is scrollable. The image should stay static. The content can be scrolled up to the status bar;
- The content in the extra carousel is scrollable if all items added to the carousel couldn’t fit into the screen. There should still be two rows, both scrolled at the same time, not separately.