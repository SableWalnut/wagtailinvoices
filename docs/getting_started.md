#Declaring models

Begin by declaring your models something like
``` python

    from django.db import models

    from wagtail.wagtailadmin.edit_handlers import FieldPanel
    from wagtail.wagtailcore.fields import RichTextField
    from wagtail.wagtailcore.models import Page

    from wagtailinvoices.models import InvoiceIndexMixin, AbstractInvoice
    from wagtailinvoices.decorators import invoiceindex


    # The decorator registers this model as a invoice index
    @invoiceindex
    class InvoiceIndex(InvoiceIndexMixin, Page):
        # Add extra fields here, as in a normal Wagtail Page class, if required
        invoice_model = 'Invoice'


    class Invoice(AbstractInvoice):
        # Invoice is a normal Django model, *not* a Wagtail Page.
        # Add any fields required for your page.
        # It already has ``date`` field, and a link to its parent ``InvoiceIndex`` Page
        full_name = models.CharField(max_length=255)
        organization = models.CharField(max_length=255)
        phone_number = models.CharField(max_length=255)
        

        panels = [
            FieldPanel('full_name', classname='full'),
            FieldPanel('organization'),
            FieldPanel('phone_number')
        ] + AbstractInvoice.panels

        def __unicode__(self):
            return self.full_name
```

#Defining settings
Define the following settings where your settings are located 


`WAGTAIL_INVOICES_ADMIN_EMAIL = '(admin to receive invoice notifactions for paid invoices etc)`
`WAGTAIL_INVOICES_VALIDATION = '' See [Custom validation and overriding](https://wagtailinvoices.readthedocs.org/en/latest/advanced/#custom-validation-and-overriding) for more information

```

There is future scope to have this setting included with [wagtailsettings](https://bitbucket.org/takeflight/wagtailsettings)