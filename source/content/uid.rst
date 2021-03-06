==============================
Content identification (ids)
==============================

.. admonition:: Description

    Different ids, UIDs, integer ids or whatever can identify your Plone
    content and give access to it.

.. contents:: :local:

Introduction
=============

Id
====

Content id generally refers the item id **within the folder**. Together with folder path this 
identifies the content in unique way.

Naturally, this id changes when the content is renamed or moved.

Use :doc:`traversing </serving/traversing>` to resolve object by path+id.

UID and UUID
=============

UID is a unique, non-human-readable identifier for a content object which stays
on the object even if the object is moved.

Plone uses UUIDs for

* Storing content-to-content references (Archetypes, ReferenceField)

* Linking by UIDs (Kupu) - this enables persistent links even though the object is moved

* Plain UID is supported by Archetypes only and is based on reference_catalog

* UUID is supported by Archetypes and Dexterity both and you should use this for new projects

UIDs are available for Archetypes content and unified UUIDs for both Archetypes and
Dexterity content items since ``plone.app.dexterity`` version 1.1.

.. note ::

	If you have pre-Dexterity 1.1 content items you must run a migration step in portal_setup to 
	give them UUIDs.

To get object UUID you can use `plone.app.uuid <http://pypi.python.org/pypi/plone.app.uuid/>`_ package.

Getting object UUID::

    from plone.uuid.interfaces import IUUID

    # BrowserView helper method
    def getUID(self):
        """ AT and Dexterity compatible way to extract UID from a content item """
        # Make sure we don't get UID from parent folder accidentally
        context = self.context.aq_base
        # Returns UID of the context or None if not available
        # Note that UID is always available for all Dexterity 1.1+
        # content and this only can fail if the content is old not migrated
        uuid = IUUID(context, None)
        return uuid

Looking up object by UUID:

Use `plone.app.uuid.utils.uuidToObject <https://github.com/plone/plone.app.uuid/blob/master/plone/app/uuid/utils.py>`_::

	from plone.app.uuid.utils import uuidToObject

	...
	obj = uuidToObject(uuid)
	if not obj:
	    # Could not find object
	    raise RuntimeError(u"Could not look-up UUID:", uuid)


More info:

* http://stackoverflow.com/questions/8618917/portal-catalog-unique-ids-for-both-archetypes-and-dexterity-content

intids
========

Integer ids ("intids") are fast look-up ids provided by ``plone.app.intid``
and ``five.intid`` packages.  Instead of relying on globally unique
identifier strings (UIDs) they use 64-bit integers, making low-level
resolution faster.

* https://github.com/plone/plone.app.intid

* http://stackoverflow.com/questions/8629390/how-to-use-intids

Archetypes
===========

This info only for Plone 3.x projects.

Getting the object's UID
===========================

Use UID() accessor function

Example how to get UID of *events* folder::

	>>> site.events.UID()
	'ce380ef0f10a85beb864025928e1819b'


Looking up object by UID
===========================

Use lookupObject() in reference catalog.

	>>> site.reference_catalog.lookupObject('ce380ef0f10a85beb864025928e1819b')
	<ATBTreeFolder at /test/events>

*None* will be returned if there is content item with matching UID (the item has been deleted).









