Unlocking bootloader on Xiaomi devices is unreasonably hard. But I managed to unlock Redmi12C in 2 days, I am gonna explain how I did it so you don't have to play this guessing game.

# Accounts Creation
Go to **Settings > Xiaomi Account** and sign up. You cannot choose global region here, but I recommend to put **United States** (though choosing Russia didn't make much difference for me)

After this open **Xiaomi Community app** and when it asks you how you want to sign in choose **"Another Account"** and **log into your Xiaomi Account**.
then tap on **ME > Set up > Change region and set it to Global**.

Before you move onto next step **account must be at least 30+ days old to avoid errors**.

# Xiaomi Community Quota Hell
First you will need to open permission for unlocking bootloader, go to **Settings > About Phone** and **tap on OS version 7 times** which will **unlock Developer Mode**. Then leave in main **Settings > Developer options** and **enable OEM unlocking with USB debugging**. <br>
(OEM stands for Original Equipment Manufacturer, in this case Xiaomi. By enabling it you are allowing your system to be changed. USB debugging let's device communicate directly with PC through USB cable, which we will need for Mi Unlock Tool to detect your Xiaomi and future customization)

Main roadblock you will face is sending request at right time in Xiaomi Community to just be even able to be granted permission to unlock bootloader.
To find menu where you can send request navigate through **ME > Unlock bootloader** and there you will find "Apply for unlocking" button.

**Every day at 0:00 in Beijing time (GMT+8) quota will reset**. Only 2000 devices can apply filling quota fast, and when I mean fast I mean FAST. We're talking *milliseconds*.
This comes down to their poor design choices. They could've made that every different model would have separated quota, but instead they made that EVERY model applies to same quota. So you're essentially playing "Hunger Games" with all of the Xiaomi users.

There's ways to combat this though.
* **Automated requests from PC**. This python script sets up Google Chrome and FireFox where you log in into your Xiaomi account so it can get Cookies to send requests to their servers. [You can get it from this repo](https://github.com/matedon/xiaomi_bypass_bootloader_unlock_quota) (note that you will need to download python3, Google and FireFox for it to function)
* **Manual tapping**. In case automated script doesn't work, open precise timer in separate tab on PC like [time.is](https://time.is/) and click on "Apply for unlock" on the phone right when it shows 23:59:59 in Beijing time.
Run both of this methods for about 5-10 seconds. After that you can stop as quota will almost certainly has been filled.

**After tapping on "Apply for unlock" you will see 1 of 2 messages**.
"You have been granted access to unlock until Beijing time (GMT+8) (mm/dd/yyyy)"
or
"Application quota limit reached. Please try again after (mm/dd) 00:00 Beijing time (GMT+8)"
Which one of this pops up **does not matter**, as their apps are incredibly buggy. I got failure message but it still worked.
So you can move to next step to actually find out if you have been granted the access.

# Mi Unlock Status
Leave into main Settings menu and **write in search bar "Mi Unlock status"** and tap the relevant option. You will see a list of rules that need to be done to add account and device, you probably did most just by using the phone so most important step is "2. Turn off Wi-Fi  and turn on mobile data. Make sure your device can connect to the internet using mobile data."
This step is **necessary**, so **make sure your mobile data connects to the internet**. Then you can press "Add account and device", which might display one of these messages

**Success**:
1. "Added successfully. Mi account is associated with this device now."

**Errors**:
1. "Could not add. Go to Mi Community to apply for authorization again."
  Most common one, this means that you couldn't reach for quota in right time.
2. "Couldn't verify, try adding your account and device again."
  This happens when it fails to reach Xiaomi's servers, usually because Wi-Fi is enabled instead of mobile data.
3. "The system is being upgraded. Please try again later"

But **These error messages don't really matter** much either.
If your mobile data was enabled instead of Wi-Fi and it still showed "Couldn't verify, try adding your account and device again." when you pressed the button it's because 2nd error might be also associated with phones settings not meeting Xiaomi's 'secure' & regional standards**, here's what can help you bypass it.
* Log out of Xiaomi account in Settings & clear it's cache
* Log out of Xiaomi Community & clear it's cache
* Restart device
* Go to **Settings > Additional settings > Region**, and change it to **United States**.
* In Settings search bar write "Find device" and enable it.
* In Settings search bar write "Xiaomi Cloud" and enable it too (you can disable sync it enables for storing contents of Notes, Recorder, Gallery... and etc. on Xiaomi Cloud)
* Restart device again for good measure

Log back into your Xiaomi account in Settings and Xiaomi Community account in their app. Go back to Mi Unlock status (don't forget to turn off Wi-Fi and turn on mobile data) and press "Add account and device". Which after it should *most likely* show success message.

> [!IMPORTANT]
> Keep in mind that after you are granted permission for unlocking bootloader you have only 336 hours (14 days) to unlock your device. <br>
> (this is unconfirmed as I couldn't find any official answer from Xiaomi itself regarding this information)

This may take a while and could get *really annoying*, ***really*** *fast*. But you should be certainly able to get it after 2 days, 1 week or a month with solid persistence and determination.

# Unlocking Bootloader
## Reducing Possibility of Soft-Bricking
* **Disable FRP** (Factory Reset Protection). It's is enabled automatically when you create or log into Google account on the phone. What it does is if someone tries to factory reset your phone (which happens in process of unlocking bootloader) FRP will trigger after the action and tell you to choose your Wi-Fi before asking to log into your Google account that was on device previously, to prove that this is you.
  The problem is how it's structured. It asks you to log into your Google account right ***after*** factory reset. If you are flashing custom ROM (like LineageOS) after unlocking bootloader your device might get soft-bricked. For example after flashing LineageOS and booting into system FRP could trigger asking you to log into previously used account, but because it's not same OS it can have bug where it wouldn't unlock even when you write correct Gmail and password.
  
  So to avoid this go to **Settings > Additional settings > Accounts & sync** then press on your Google account and tap on button with 3 dots on bottom and choose "Remove account" from menu.
  Do the same thing with all of your other Google accounts and it will say "Device protection features will no longer work." which is their way of saying that FRP will not work.

* **Disable Screen lock**. PIN is a part of FBE (File-Based Encryption). FBE works by using unique generated key to encrypt all of your data. But enabling Screen lock PIN adds another layer to this encryption, PIN acts like master key that encrypts all of your data, you cannot access almost anything before you input it in lock screen. There's no way to mechanically find PIN, only to brute force if you forgot it.
  But also it can conflict with new ROM the same way it does with FRP. It might ask you for PIN on a new OS, but even if you write the correct one it will throw an error because of internal mismatch between stock OS and custom OS.
  
  To avoid this from happening go to **Settings > Fingerprints, face data and screen lock** and press on "Screen lock", write your PIN code and choose "Turn off screen lock"

## Mi Unlock Tool
Congratulations! The agonizing part is finally over. Now we just have to deal with their agonizingly buggy tool.

You can download **Mi Unlock Tool from their** [official page](https://en.miui.com/unlock/download_en.html). But it will give you **older, buggy version** that will almost certainly not display log in screen for you. It will remind you to download an update though, but upon pressing download it will throw you into web page that is... privated.
So we're gonna have to **instead download it from** [Uptodown](https://mi-unlock.en.uptodown.com/windows). (as far as I can tell this is official upload of the tool from Xiaomi due to signature not being modified) But before opening if you extracted zip of old Mi Unlock tool **delete it's extracted files** and terminate all instances of old Mi Unlock Tool in Task Manager Because they can conflict with each other.

After you will actually be able to properly open the tool, it will meet you with log in screen where you will need to enter Xiaomi account information and phone number where they will send SMS code to your phone that you will need to write in box. It may charge money from your mobile data first time due to phone sending outgoing encrypted SMS from your device to Xiaomi's servers to test integrity of your SIM card, or it might send security code to your WhatsApp if you have your phone number tied to it (this is proven to be official feature but most services don't do this, so be sure to check WhatsApp if you can't find code)
Then it will let you use the tool. To unlock the phone
* Turn off the phone
* Hold volume down and power down button until it enters FastBoot mode
* Connect phone to PC with USB cable
  (if your phone does not connect in fastboot mode, try using USB 2.0) <br>

Which after Unlock button should be lit up. When pressing it will give you warnings that you should be already aware of, and after you confirm your choice it will make you wait for 72 hours (3 days) if you have HyperOS, or 168 hours (1 week) if you have MIUI before you can actually unlock your phone. What wait time is determined from is not confirmed, but is a community observation. In my case I have HyperOS and got 72 hours.
(while you wait I recommend setting up timer on PC or phone itself with same amount of hours plus 30 minutes to know when you can unlock the device for sure)

Timer works completely on side of Xiaomi's servers so you can safely close the software, unplug the phone, and hold volume down with power down button until phone boots back in system.

> [!WARNING]
> **While you wait for timer to end, do NOT do these things**.
> Do not log out of Xiaomi account <br>
> Do not log out of Xiaomi Community account <br>
> Do not press "Add account and device" in Mi Unlock status <br>
> Do not turn off OEM unlocking <br>
> Do not pull out SIM card <br>
> Do not turn off Find device (if you turned it on) <br>
> Do not turn off Xiaomi Cloud (if you turned it on) <br>
> Do not change your region

Basically **do not switch anything related to unlocking bootloader, or the timer will reset**. Otherwise you can use your phone as usual while you wait.
