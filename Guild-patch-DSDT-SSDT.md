# NVidia Graphics Disable
This method I learn from Insanelyi mac and that guide is quite easy
if you want cakewalk method than follow my guide
- First open you DSDT.dsl
- And paste thing at top of DSDT.dsl
- `External (_SB_.PCI0.PEG0.PEGP._PS3, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._PS0, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._OFF, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._ON, MethodObj)
External (_SB_.PCI0.PEG0.PEGP.SGOF, MethodObj)
External (_SB_.PCI0.PEG0.PEGP.SGON, MethodObj)`
-  Then Find this Method (_WAK
Add this before

- `Method (M_ON, 0, NotSerialized){If (CondRefOf(\_SB_.PCI0.PEG0.PEGP._ON)){\_SB_.PCI0.PEG0.PEGP._ON()}If (CondRefOf(\_SB_.PCI0.PEG0.PEGP._PS0)){\_SB_.PCI0.PEG0.PEGP._PS0()}If (CondRefOf(\_SB_.PCI0.PEG0.PEGP.SGON)){\_SB_.PCI0.PEG0.PEGP.SGON()}}`
- `Method (M_OF, 0, NotSerialized){If (CondRefOf(\_SB_.PCI0.PEG0.PEGP._OFF)){\_SB_.PCI0.PEG0.PEGP._OFF()}If (CondRefOf(\_SB_.PCI0.PEG0.PEGP._PS3)){\_SB_.PCI0.PEG0.PEGP._PS3()}If (CondRefOf(\_SB_.PCI0.PEG0.PEGP.SGOF)){\_SB_.PCI0.PEG0.PEGP.SGOF()}}`
- Then put M_OF in between Method (_INI), (_wak) like this . EG:-
-
      Method (_WAK, 1, Serialized)

         {
           M_OF ()
         If (LAnd (LEqual (\_SB.PCI0.LPCB.EC0.AAST, One), LEqual (\_SB.PCI0.LPCB.EC0.AAEN, One)))
         {
             Store (Zero, GP53)

-

         Method (_INI, 0, NotSerialized)
         {
             Store (0x07D0, OSYS)
             M_OF ()
             If (CondRefOf (\_OSI, Local0))
             {
                 If (_OSI ("Windows 2001"))

- Sleep will not work so you have to apply this
  -
  Method (_PTS, 1, NotSerialized)

     {

        M_ON ()

- just put M_ON () after opening the bracket in Method (_PTS
- Note Don't copy paste These are examples not solutions
- Finally verdict open SSDT-x 'where x can be 1-14'
- Scope (\_SB.PCI0.PEG0.PEGP)
- In my case I have SSDT-8,SSDT-9,SSDT-10 and in these APPlY RehabMan Laptop patch GFX0 -> IGPU
- Then save as ACPI file NOT DSL
- Then copy to your EFI/CLOVER/ACPI/PATCHED
SUCCESSfully you patched and disable Nvidia graphics

# Fix Audio
- Open Open DSDT using 'Macial'
- Apply patch IRQ Fix
- Apply patch Audio layout 3
- Find Device (HDEF)  -> Method (_PRW, 0, NotSerialized)  // _PRW: Power Resources for Wake
- Chage Return (GPRW (0x0D, 0x04)) to Return (GPRW (0x03, 0x04))
- Save DSDT and put in /EFI/CLOVER/ACPI/Patched
- Nice!

# Fix Brightness

- Open Open DSDT using 'Macial'
- Apply patch Brightness fi (HD3000/HD4000)
- Save DSDT as DSDT.aml and put in /EFI/CLOVER/ACPI/Patched
- Nice!