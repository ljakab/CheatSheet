# Highlight whitespace

General -> Editors -> Text Editors -> Show whitespace characters

# Avoid trailing whitespace

  * Java -> Editor -> Save Actions
  * check "Perform the selected actions on save" and "Additional actions" and
    uncheck the rest
  * click "Configure..." next to the additional actions (expand the form's
    window if you don't see the button)
  * Check "Remove trailing whitespace" and "All lines" on the first tab ("Code
    Organizing"), uncheck everything else on the other tabs (at the bottom of
    the form you should see "1 of 27 save actions activated"; the "1" in the
    message is important, the "27" can differ if not in version 4.4)

# Don't automatically add stuff to .gitignore

Team -> Git -> Projects -> Automaticaly ignore derived resouces by adding them
to .gitignore (unchecked)

# Import a downloaded custom formatter (e.g., OpenDaylight)

Java -> Code Style -> Formatter -> Import...

(https://git.opendaylight.org/gerrit/gitweb?p=odlparent.git;a=blob_plain;f=checkstyle/src/main/resources/odl_checks.xml)
