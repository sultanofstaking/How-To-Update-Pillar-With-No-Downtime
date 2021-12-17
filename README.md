# How-To-Update-Pillar-With-No-Downtime

**WARNING: I created this guide as an experiment for those wanting to update their pillar with no downtime. The steps below are my own and not endorsed by the Zenon team. This is being done in a very agile way, MVP of an experiment I expect our cross-functional community will participate and provide feedback so we can continuously improve. Please use the issue feature for fixes here. If you need to reach me 1:1 DM me on TG @SultanOfStaking - Try at your own risk.**

### Summary
Pillar updates are necessary, downtime isn't. In it's simplest form this guide walks you through a setup where you run two pillars. Pillar 1 connected to your syrius wallet that is producing momentums and pillar 2 that is not. You will perform all updates on pillar 2 first, confirm everything works, swap producer addresses in syrius to pillar 2, update pillar 1, then swap producer addresses back to where you started. Performing updates in this manner should reduce the risk that you miss any momentums because of downtime. This set-up will cost a bit more, but you will be thankful it is in place should an update not go as expected.

### Steps

**Step 1: Launch a clone pillar**

NOTE: Steps 1 and 2 are done completely in virtual machines. Do not touch syrius for this step!

The clone pillar (pillar 2) should have the same exact specs as your primary pillar (pillar 1). That means same service provider, same OS, same CPU, Memory, etc. It is fine to put the clone in another region (in fact encouraged) but everything else should be the exact same. The point is to be sure the update doesnt break your machine so the spects need to be the same otherwise you are introducing variables.

VPS is up and we are ready to launch the pillar right? No. Double check everythign is set up on the new VPS exactly as it is on pillar 1 VPS. SSH Config, UFW rules, any monitoring services etc. Follow the exact same steps to set up the new VPS as you did to set up pillar 1 VPS or again, you are introducing variables.

Once pillar 2 VPS is set up exactly the same as pillar 1, follow the the teams launching guide for pillar deployment https://github.com/zenon-network/znn-bundle/blob/master/PILLARS.md or use the SultanOfStaking guide https://github.com/sultanofstaking/Zenon-Pillar-Deployment 

Run logs to be sure it is at the current height. Since this pillar is not active you will see it only inserts, not produces momentum.

`sudo journalctl -f -u go-zenon`

Check status and find your producer address.

`./znn-controller`

Select `3` for status and you should see your producer address in green.

**Step 2: Test your updates on Pillar 2**

If the team says you must update your pillar, run the update on pillar 2 first. The best way to do this is to have 2 terminals up. One should be where you will enter the commands and one should be running logs with the command above. Follow the teams or community guide for updating your pillar on pillar 2 and be sure that it continues to insert blocks as expected. At the time of writing many people will be "sentrifying" their pillars. This is a great example of where this setup can help since sentriy adjust firewalls. If you test sentrify on pillar 2, and for some reason the update fails or your pillar stops inserting blocks, you can now troubleshoot pillar 2 without worrying about missing momentums on pillar 1.

Hopefully the update went as expected. If not troubleshoot it until everything looks normal in pillar 2's logs.

**Step 3: Switch producer address in syrius to pillar 2**

Before completing this step it is a good idea to have a terminal up showing logs for both pillar 1 and pillar 2. You will see pillar 1 is producing momentum periodically and pillar 2 is only inserting momentum.

Go to the pillar tab in syrius, update pillar, and change the producer address to the producer address associated with pillar 2. Monitor logs and you should see that pillar 2 will now be producing momentum and pillar 1 is only inserting momentum.

**Step 4: Update and revert back to pillar 1**

Now that pillar 2 is producing momentum you can safely update pillar 1. Do the same steps for the update, ensure it is working as intended via logs, and when you are ready go back into syrius and change the producer address back to pillar 1. Monitor logs and you will see pillar 1 producing momentum after a short period.

## Congrats! If you followed this guide your stress level should go down quite a bit when updating your pillar!
