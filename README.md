--- To Save contact data into data base using aura components ???

--- Application
<aura:application extends="force:slds">
    <c:LC_dynamicRecordRowsCreation />
</aura:application>

--- Apex Controller
public class CreateContactcls1 {
	@Auraenabled
    public static string createContact(Contact contObj){
        System.debug('contact::'+contObj.firstName);
        insert contObj;
		return contObj.Id;        
    }
    @Auraenabled
    public static List<Contact> retrieveContacts(){
        
        return [select id,FirstName,LastName,email from Contact order by createdDate desc limit 5];
    }
}

--- Component
<aura:component controller="CreateContactcls1">
    <aura:attribute name="contactObj" type="Contact" default="{'sobjectType':'Contact',
                                                             'FirstName':'',
                                                             'LastName':'',
                                                              'Phone':''}"/>
    
    <aura:attribute name="contactId" type="String"/>    
    <aura:attribute name="contactList" type="Contact[]"/>
    <aura:handler name="init" value="{!this}" action="{!c.showContacts}"/>
    <article class="slds-card">
        <div class="slds-card__header slds-grid">
    <header class="slds-media slds-media_center slds-has-flexi-truncate">
      <div class="slds-media__figure">
        <span class="slds-icon_container slds-icon-standard-account" title="account">            
          <lightning:icon iconName="standard:contact" alternativeText="Contact" />
        </span>
      </div>
      <div class="slds-media__body">
        <h2 class="slds-card__header-title">
          <a href="javascript:void(0);" class="slds-card__header-link slds-truncate" title="Accounts">
            <span>Contact</span>
          </a>
        </h2>
      </div>
      <div class="slds-no-flex">
        <lightning:button variant="brand" label="Save" title="Save" onclick="{! c.doSave }" />
      </div>
    </header>
  </div>        
    	<div class="slds-card__body  slds-card__body_inner">
        	<lightning:input value="{!v.contactObj.FirstName}" label="First Name" placeholder="enter first Name..."/>
        	<lightning:input  value="{!v.contactObj.LastName}" label="Last Name" placeholder="enter last Name..." />
    		<lightning:input  value="{!v.contactObj.Phone}" label="Phone" placeholder="enter phone..."/>
        	
            <div><br/>
                <table class="slds-table slds-table_cell-buffer slds-table_bordered">
                   <thead>
                        <tr>
                            <th class="slds-text-title_caps" scope="col">                                
                                <div class="slds-truncate" title="First Name">First Name</div>
                            </th>
                             <th class="slds-text-title_caps" scope="col">
                                 <div class="slds-truncate" title="Last Name">Last Name</div>
                           	</th>
                        </tr>
                    </thead>
                     <tbody>
                        <aura:iteration items="{!v.contactList}" var="contObj"  indexVar="index"> 
                            <tr>
                            	<td data-label="First Name" scope="row">
                                    <div class="slds-truncate" title="FirstName">{!contObj.FirstName} </div>
                                </td>
                                <td data-label="Last Name">
                                    <div class="slds-truncate" title="LastName">{!contObj.LastName}</div> 
                                </td>
                            </tr>
                            
                        </aura:iteration>
                    </tbody>
                </table>
            </div>
        </div>
        <footer class="slds-card__footer">
            <div class="demo-only" style="height: 4rem;">
  <div class="slds-notify_container slds-is-relative">
    <div class="slds-notify slds-notify_toast slds-theme_success" role="status">    
     
        <lightning:icon iconName="utility:success" alternativeText="success"  aria-hidden="true"/>
    
      <div class="slds-notify__content">
        <h2 class="slds-text-heading_small "><a href="javascript:void(0);">Contact Id :{!v.contactId}  </a> contact was created.</h2>
      </div>
      <div class="slds-notify__close">
        <button class="slds-button slds-button_icon slds-button_icon-inverse" title="Close">
            <lightning:icon iconName="utility:close" alternativeText="close"/>
        </button>
      </div>
    </div>
  </div>
</div>
    				  
  		</footer>
    </article>
</aura:component>

--- Controller JS
({
	doSave : function(component, event, helper) {
		var action = component.get("c.createContact");
        var contactList = component.get("v.contactList");
        action.setParams({'contObj':component.get('v.contactObj')});        
        action.setCallback(this,function(data){           
            component.set('v.contactId',data.getReturnValue())
            contactList.splice(0,0,component.get('v.contactObj'));
            component.set("v.contactList",contactList);     
        });        
        $A.enqueueAction(action);
	},
    showContacts : function(component, event, helper) {       
		var action = component.get("c.retrieveContacts");        
        action.setCallback(this,function(data){            
            component.set('v.contactList',data.getReturnValue())
        });
        
        $A.enqueueAction(action);
	},
    
})

  ----------------------------------------------------------------------------------------------------------------------
  
--- Save Contacts using aura components ???
  
--- Application
<aura:application extends="force:slds">
    <c:LC_ContactCreation />
</aura:application>

--- Apex Controller

public class CreateContactcls {
	@Auraenabled
    public static string createContact(Contact contObj){
        System.debug('contact::'+contObj.firstName);
        insert contObj;
		return contObj.Id;        
    }
    @Auraenabled
    public static List<Contact> retrieveContacts(){
        
        return [select id,FirstName,LastName,email from Contact limit 10];
    }
}

--- Component
<aura:component controller="CreateContactcls">
	 <aura:attribute name="contactObj" type="Contact" default="{'sobjectType':'Contact',
                                                             'FirstName':'',
                                                             'LastName':'',
                                                              'Phone':''}"/>
    
    <aura:attribute name="contactId" type="String"/>    
    <article class="slds-card">
        <div class="slds-card__header slds-grid">
    <header class="slds-media slds-media_center slds-has-flexi-truncate">
      <div class="slds-media__figure">
        <span class="slds-icon_container slds-icon-standard-account" title="account">            
          <lightning:icon iconName="standard:contact" alternativeText="Contact" />
        </span>
      </div>
      <div class="slds-media__body">
        <h2 class="slds-card__header-title">
          <a href="javascript:void(0);" class="slds-card__header-link slds-truncate" title="Accounts">
            <span>Contact</span>
          </a>
        </h2>
      </div>
      <div class="slds-no-flex">
        <lightning:button variant="brand" label="Save" title="Save" onclick="{! c.doSave }" />
      </div>
    </header>
  </div>        
    <div class="slds-card__body  slds-card__body_inner">
        	<lightning:input value="{!v.contactObj.FirstName}" label="First Name" placeholder="enter first Name..."/>
        	<lightning:input  value="{!v.contactObj.LastName}" label="Last Name" placeholder="enter last Name..." />
    		<lightning:input  value="{!v.contactObj.Phone}" label="Phone" placeholder="enter phone..."/>
        	
        </div>
        <footer class="slds-card__footer">
            <div class="demo-only" style="height: 4rem;">
  <div class="slds-notify_container slds-is-relative">
    <div class="slds-notify slds-notify_toast slds-theme_success" role="status">    
     
        <lightning:icon iconName="utility:success" alternativeText="success"  aria-hidden="true"/>
    
      <div class="slds-notify__content">
        <h2 class="slds-text-heading_small "><a href="javascript:void(0);">Contact Id :{!v.contactId}  </a> contact was created.</h2>
      </div>
      <div class="slds-notify__close">
        <button class="slds-button slds-button_icon slds-button_icon-inverse" title="Close">
            <lightning:icon iconName="utility:close" alternativeText="close"/>
        </button>
      </div>
    </div>
  </div>
</div>
    				  
  		</footer>
    </article>
</aura:component>

--- Controller JS
({
	doSave : function(component, event, helper) {
		var action = component.get("c.createContact");
        action.setParams({'contObj':component.get('v.contactObj')});
        action.setCallback(this,function(data){           
            component.set('v.contactId',data.getReturnValue())
        });        
        $A.enqueueAction(action);
	}
})

  -------------------------------------------------------------------------------------------------------------------------
--- Accounts display records using aura components ???
  
--- Application
<aura:application extends="force:slds">
    <c:LC_LightingDataTableExample />
</aura:application>

--- Apex Controller
public class AccountListController1 {
	@AuraEnabled
    public static List < Account > fetchAccts() {
        return [ SELECT Id, Name, Industry, Type FROM Account LIMIT 30];
    }
}

--- Component
<aura:component controller="AccountListController1">
	<aura:attribute type="Account[]" name="acctList"/>
    <aura:attribute name="mycolumns" type="List"/>
     <aura:handler name="init" value="{!this}" action="{!c.fetchAccounts}"/>
    <lightning:datatable data="{! v.acctList }" 
                         columns="{! v.mycolumns }" 
                         keyField="id"
                         hideCheckboxColumn="true"/>
</aura:component>

--- Controller JS
({
	fetchAccounts : function(component, event, helper) {
        component.set('v.mycolumns', [
            	{label: 'Name', fieldName: 'linkName', type: 'url', typeAttributes: {label: { fieldName: 'Name' }, target: '_blank'}},
                {label: 'Industry', fieldName: 'Industry', type: 'text'},
                {label: 'Type', fieldName: 'Type', type: 'Text'}
            ]);
		var action = component.get("c.fetchAccts");
        action.setParams({
        });
        action.setCallback(this, function(response){
            var state = response.getState();
            if (state === "SUCCESS") {
                var recordList = response.getReturnValue();  
                recordList.forEach(function(record){
                    record.linkName = '/'+record.Id;
                });
                component.set("v.acctList", recordList);
            }
        });
        $A.enqueueAction(action);
	}
})



