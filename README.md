# goteti-forms-discussions
goteti-forms  : Discussions on new features , ideas, report issues and so on.


# Goteti Forms  

Author: Narasimha Goteti 

"GotetiForms" library is for the Angular (2+) project development. 

## Discussions / Issue tracker:

https://github.com/gsnr-narasimha-edu/goteti-forms-discussions/issues

### Updates:
> GtFormStatus directive, GtFormStatusService, GtLoadComponent directive, GtLoadTemplate directive, GtRef directive added

> "errorsText" property available for GtControl, [ OtherData ] attribute available for GtFormStatusDirective. [ formStatus ] attribute available for gt-error-msg component, GtFun pipe that accepts function as angular template pipe.

> Removed templates Input binding for gt-reactive-form and gt-input-field, instead use GtFormStatus directive (Refer )

>  Removed support for auto-select

>  'skip' control in gt-reactive-form  functionality supported.

> "GtFormHub" class added, Support added for Async Validators.
(Refer: B1.1 Implementation)

> 'commonInputTemplate' support added for the gt-reactive-form (Refer B1.2.1 Implementation)

> control attribute added to 'gt-error-msg' component for rendering the AbstractControl errors (Refer: B1.2.3).

> Added {type: 'template', template: 'customtemplate' } support for the GtGroupBP & GtArrayBP, so that custom templates used for GtGroup, GtArray while rendering in gt-reactive-form .

> Added additional properties to GtControl, GtGroup, GtArray (Refer: B1.2.4).


> "GtCustomRulesService" support removed.

> GtFormHub, gtFormBuilder, gtUpdateControl  supports 'AbstractControlOptions' i.e {updateOn: 'blur'}.

> In upcoming versions: gtFormBuilder, gtUpdateControl will be removed instead use 'GtFormHub' (build, update methods in GtFormHub) ;

### NOTE: Versions 5+ are re-written which have new structures and new configurations so not compatible with previous versions. 

### Caution: Information about version 4.
1. type : 'radio' needs 'list' property as array of objects with (label, value) properties ex: list: [{ label: 'Yes', value: true}, {label:'No',value: false}]
2. All services , components selectors and names are changed.
3. if type:'auto-select' then list: [{id:1,...otherProperties}] . 'id' property is mandatory and unique value.


## A1) Integration Steps:

Install package in the root folder using the command,

    npm i goteti-forms

Add the module to the NgModule of main application.

    import { GotetiFormsModule, GtFormsService, GtFormHub} from 'goteti-forms';
    @NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule,
        AppRoutingModule,
        FormsModule,
        GotetiFormsModule  // <--- Add the Module 
    ],
    providers: [],
    bootstrap: [AppComponent]
    })
    export class AppModule {
        constructor (private GFS: GtFormsService){
            GFS.addComponent(<custom_componet_key>, <Your Custom Input Component>);
        }
    }

Module contains the following:

    import {
        AtbrDirective, DebounceInputDirective, FilterPipe, GtAutoSelectComponent, GotetiFormsModule, GtFormsService, GtFormHub, GtInputFieldComponent, GtRecursiveFormControlComponent, GtErrorMsgComponent, GtMapBuilder, HighlightSearch, IpolatePipe, ListenerDirective, PermissionDirective, PermissionDirectiveDisable, SetFocusDirective, gTrefDirective, gtLoadTemplate, GtLoadComponentDirectives, gtFormBuilder, gtFormControl, gtFormGroup, gtFormArray, gtAddToFormArray, gtSetValidator, sampleReactiveFormData
    } from 'goteti-forms';

## A2) Details of all the Components / Pipes / Directives / Utils.

### A2.1)  Component : GotetiInputFieldComponent
    
Example:


    // Supports both Template driven and Reactive forms
    
    //app.component.html 
        
        <gt-ifield 
            [config]="configuration" 
            [listen]="{input: onInputValue, change: onChangeValue}"
            [args]="'Put the customised arguments here'"
            [(ngModel)]="title" required>
        </gt-ifield>

    //app.component.ts
    ngOnInit(){
        this.configuration =  {
                "label": "Name",
                "type": "text",
                "template": null,  
                "class": "form-control",
                "multiple": true,
                "listValue": "capital",
                "disable": false,
                "hide": false,
                "itpolate": "{name}'s last Name is {lastname} !",
                "list":[{id:1, "name":"Narasimha","lastname":"Goteti"},
                        {id:2,"name":"Sai","lastname": "Vutukuru"}],
                "atbr": {
                  "placeholder": "It's upto you! "
                },
                "errors":{
                    "required":{
                        "msg": "Name is required",
                        "class": "text-warning"
                    }
                },
                "validations": [{
                  "key": "minLength",
                  "asyncfn" : false,  /* <== This value should be true for custom Async validators else false */
                  "value": 5
                },{
                  "key": "required",
                  "value": 5
                }]
              };        

    }

    onInputValue(event, args){
        console.log(event) // event, 'Put the customised arguments here' 
    }

    onChangeValue(event, args){
        console.log(event) //// event, 'Put the customised arguments here' 
    }

Properties of [config]:

 | Properties| Description| Values Supported / Example | 
 |-|-|-|
 | label | Label  for Input field |  String , null |
 | heading| It is for gt-reactive-form heading with index value| heading: "{index} I am Header for a form group"|
 | class | Class for the Input field | ex: 'form-control classname' |
 | disable |  If it is 'true', input will be in disable state , Even modifing the attributes from developer tools (inspect elements ) will not work i.e still input field will be disable state| true / false|
 |hide| It is NOT CSS based it is *ngIf based. | true / false|
 | value | Value of input |  |
 | type  | All basic input types additional to Library input types  i.e (auto-select, fnumber), (This DO NOT support 'select','template' types).  For type:'radio', list:[{label:'Yes', value: true},{label:'No',value:false}] format is required.    | auto-select, template, component, textarea, text, number and (all input types)  |
 | multiple | 'true' for multi-select , 'false' for single select. multiple true works only for  type='auto-select'.   |  true / false | 
 | listValue | type="auto-select" selected value should be property or whole selected object of array iteration| Property key or null |
 | itpolate |  type="auto-select", It is string with {key} ,format of displaying data in dropdown.| 'My name is {name} and last name is {lastname} !'|
 | list | type="auto-select", Array of Objects (example: {type:'auto-select', listValue:'value',list:[{id:1, label:'India',value:'IND'}]) or Array of non-objects for dropdown | Array of objects('id' property is mandatory which is unique.), Array of non-objects |
 | atbr | Attributes for input field | ex: {step: 10}|
 | random | Random number has to be updated when ever atbr value is updated inorder to update the Input field's attribute. | Math.random() |
 | controlOptions | {updateOn:'submit'} (change/blur/submit)| |
 | errors| Error messages which displays in input field | |
 | validations | Validation object value for Reactive form which supports both sync & async validators| <pre>[{ key: 'customValidator', asyncfn: false, value: 'test'},{key:'customAsyncValidator', asyncfn: true, value: 'test'}]</pre>|
 | inputs | For type component, inputs is the keyValue pairs that acts like Input bindings to component| <pre>ex: {type: 'component', component:'< component that added to GtFormService >', inputs:{title: '@input binding title of the component'}}</pre>||
||||

## A2.2) Supported 'type' values
   HTML types:  text , number , range ... etc

   | type | Description | example |
   |-|-|-|
   | < any html input type> | like type is 'text' or 'number' or 'checkbox' ..etc ... | {type:'text', label:'Firsta Name', name:'firstname',class:'form-control',divclass:'col-md-3',disable:false, hide: false,atbr:{placeholder:'Name'}, validations:[{key:'required'}], errors: {required:{msg:'First Name is required'}}}|
   |radio|config should contain 'list' property with array of objects (label and value ) format .|list: [{label:'yes',value: true},{label:'no',value: false}]|
   |select| NOT-SUPPORTED| NOT-SUPPORTED|

   Library supported types : 
   
   |type| Description| example |
   |-|-|-|
   |auto-select (removed)| 1. If list is array of objects ('id' property with unique values are mandatory), If 'listValue' property is present result value will be 'listValue' mentioned value else result will be whole selected object. <br> 2. If list is array of values , then no need of 'listValue' property. |<b>Array of objects</b><br><pre> {type:'auto-select',multiple: false,list:[{id:1,label:'India', value:'IND'},{id:1,label:'USA', value:'US'}], listValue:'value', itpolate:'{label} shortcode is {value}'} </pre><br><br> <b> Array of values</b> <br> <pre> {type:'auto-select',multiple: true,list:['India', 'USA'], listValue:'value'}</pre>|
   |component| If type is component , then component property with custome component name is mandatory. You can add your additional properties required for the custom component. Custom component should be added to the 'GotetiFormsService' service. <br>For reference please follow the below 'component' example.| {type:'component', component:'toggle', customeproperty:'customepropertyvalue'}|
   |template|For reference please follow the below 'template' example.||



   auto-select (ex:{type:'auto-select',list:[{id:1, label:'India',value:'IND'}]), component, template .
   
Properties of [listen]:

listen attribute accept the object with key and value, where key is valid input events like 'input','change','keydown'... etc , value is the function which needs to handle in '.ts' file.

Properties of [args]:

args=Arguments  of listen events, [listen] attribute is mandatory for this , so that arguments will be passed to listen events like 'input, change, keydown,keypress'. 

'args' can be String, Array, Object.

# A2.3) type:'component' Implementation
If type:"component" follow the below structure and Make sure you add the custome input component to the GotetiFormsService.

    // app.module.ts
    export class AppModule {
        constructor (private GFS: GtFormsService){
            GFS.addComponent('toggle', ToggleInputComponent);
            GFS.formbuilder = new GtFormHub({
                availableCheck: this.availableCheck.bind(this),
                rangelimit: this.rangelimit.bind(this)
            });
        }

        availableCheck(list): ValidatorFn {
            return (control: AbstractControl): { [key: string]: boolean } | null => {
                if ([null,undefined].includes(control.value)) {
                    return { 'availableCheck': true };
                }
                return null;
            };
        }

         rangelimit(range): ValidatorFn {
            return (control: AbstractControl): { [key: string]: boolean } | null => {
                if (control.value.length < range) {
                    return { 'rangelimit': true };
                }
                return null;
            };
        }



    /* ToggleInputComponent is project level custom input component , i.e not the part of GotetiForms. Put your custom input component in place of ToggleInputComponent */ 
    /*
       this.rangelimit and this.availableCheck are custom validators in project level.
    */

    //app.component.ts
    this.configuration = {
        "label": "Contact Available?",
        "type": "component",
        "component": "toggle",
        "errors":{
            "availableCheck":{ /* availableCheck is custome validator */
                "msg": "Contact check is required",
                "class": "text-danger"
            }
        },
        "validations":[{
            key:'availableCheck',
            value: null
        }]
    }

    onChangeValue(ev){
         console.log( arguments)
    }

    // HTML implementation.
    <gt-ifield 
        [config]="configuration" 
        [listen]="{change: onChangeValue}"
        [(ngModel)]="ratingValue">
    </gt-ifield>



### Creating Custom Input Component

    @Component({
    selector: 'toggle',
    template: `
        <div>
            <label>{{config?.label}}</label>
            <span [ngClass]="{'btn': true, 'btn-primary': value}" (click)="togglev()"> ON </span>
            <span [ngClass]="{'btn': true, 'btn-primary': !value}" (click)="togglev()"> OFF </span>
        </div>
    `,
    })
    export class ToggleInputComponent implements OnInit, OnChanges{
        value;
        @Input() formControl: any; // this is FormControl object with 'config' property added to it.
        
        ngOnInit(){
            if (this.formControl && this.formControl.value){
                this.value = this.formControl.value;
            }
        }

        get config(){
            return this.formControl && this.formControl.config;
        }
      
        setvalue(value){
            this.value = val;
            if (this.formControl && this.formControl.setValue){
                this.formControl.setValue(this.value);
            }
        }
        

    }

# A2.4) type:'template'  Implementation

    //app.component.ts
    this.configuration = {
        "label": "Contact Available?",
        "type": "template",
        "template": "testtemplate",
        "divclass":"col-md-5",
        "errors":{
        },
        "validations":[]
    }

    onChangeValue(ev){
         console.log( arguments)
    }
    
    gtFormHub = new GtFormHub({customeValidator: Function})

    this.gtFormStatus = {
        submitted: false,
        checkOnSubmit: true,
        checkOnDirty: false,
        checkOnTouch: false,
        gtErrorsView: true, /* this will show default message view if you want to customize the errors views make this false and create your own view */
        showAllErrors: false, /* Default its true, when it is true, all error messages will shown ingoring checkOnSubmit,checkOnDirty,checkOnTouch booleans*/
        formName: 'MyForm'
        onControlUpdate:function(this: any, control: any, args: any){ // This function fires on every control value updates when [(ngModel)] attribute is used.
            // console.log(control, this)

            console.log(this.gtFormStatus.isNgFormValid('valid')) // valid | touched | dirty
            
            /* isNgFormValid only works for [(ngModel)] based elements, for [formControl] use this.formGroup.valid based validations */
         }.bind(this)
    }

    //onSubmit make gtFormStatus.submitted = true

    <div [GtFormStatus]= "gtFormStatus" [templates]="{testtemplate: templatename}" [GtFormHub]="gtFormHub" [OtherData]="OtherData">
    // HTML implementation.
        <gt-ifield 
            [config]="configuration" 
            [listen]="{change: onChangeValue}"
            [args]="'customeargs'"            
            [(ngModel)]="ratingValue">
        </gt-ifield>
    </div>
    <ng-template #templatename let-control="control" let-config="config" let-formStatus="formStatus" let-listen="listen" let-args="args" let-all>
        /* control is the formControl or NgModel object with config property.*/
        <button *ngFor="let l of control?.config?.list" (click)="control.setValue(l.value)">{{l.label}} </button>
    </ng-template>

                                                        (OR)
    <ng-template gtref="templatename" let-control="control" let-config="config"  let-formStatus="formStatus" let-listen="listen" let-args="args" let-all>
        /* control is the formControl or NgModel object with config property.*/
        <input [formControl]="control"/>
    </ng-template>

                                                        (OR)

    <ng-template #templatename let-listen="listen" let-config="config"   let-formStatus="formStatus" let-args="args" let-valueModel="valueModel" let-disabled="disabled" let-all>
        /* valueModel.update is the function which updates the value of NgModel.*/
        <button *ngFor="let l of control?.config?.list" (click)="valueModel.update(l.value)">{{l.label}} </button>
    </ng-template>  
                                                (OR)
      <ng-template gtref="templatename" let-config="config"  let-formStatus="formStatus" let-listen="listen" let-args="args" let-valueModel="valueModel" let-disabled="disabled"  let-all>
        <input [(ngModel)]="valueModel.value" />
       
    </ng-template>                                                       




## 'GtFormsService' methods:
    getComponent(key)
    getAllComponents()
    addComponent(name, component)
    removeComponent(key)
    setComponents(componentlist)
    getTemplate(key)
    getAllTemplates()
    addTemplate(name, Template)
    removeTemplate(key)
    setTemplates(Templatelist)

## 'GtFormStatusService':
    FormStatus:GtFormStatusInterface = {}; 
    FormHub?: GtFormHub = undefined;
    getComponent(key)
    getAllComponents()
    addComponent(name, component)
    removeComponent(key)
    setComponents(componentlist)
    getTemplate(key)
    getAllTemplates()
    addTemplate(name, Template)
    removeTemplate(key)
    setTemplates(Templatelist)


    Note1: GtFormStatusService scope is inside GtFormStatus directive.
    Note2: Template/component search order is GtFormStatusService if not found then searches in  GtFormService
    Note3: If you use <ng-template [gtref]="<customTemplate>">...</ng-template> , then this customTemplate Ref will be added to GtFormService.

# B1) GtFormHub 
class methods: (alternative to GtFormBuilder & gtUpdateControl)

    1. constructor(<custom Validators as object of functions>)
    2. build(blueprint) /* alternative to GtFormBuilder, blueprint is configuration as shown in above examples */
    3. update(updatedObject, blueprint)  /* alternative to gtUpdateControl */
    
### B1.1)   'GtFormHub' Implementation:
<pre>
    let condition = true;
    let gtformhub = new GtFormHub({
        'customValidator': function(args){
            return ((c: AbstractControl) => {
                    return  condition ? {
                        customValidator: true
                    } : null; 
                })
            }.bind(this), 
            /* binding the component's instance i.e 'this' is important*/

        'customASYNCValidator':  function(args){
            return ((c: AbstractControl) => {
                    return Promise.resolve(condition ? {
                         customASYNCValidator: true 
                         } : null); 
                })
            }.bind(this) /* binding the component's instance i.e 'this' is important */
        }
    });

    let blueprintConfig: GtControlBP  = {
        type : 'input',
        placeholder: 'Name',
        controlOptions: {
            updateOn: 'change'
        },
        validations : [{
            key: 'customValidator',
            value: 'args'
        }, {
            key: 'customASYNCValidator',
            asyncfn: true,
            value: 'args'
        }]
    };

    /* Creation of Form Builder instance */

    let inputcontrol = gtformhub.build(blueprintConfig); /* Returns the Form Control with config property */

                        (OR)

    /* Creation of Form Builder instance with existing value */

    let inputcontrol = gtformhub.update('Goteti',blueprintConfig);
                       

    /* Updation of existing Form Builder instance with new values*/
    
    gtformhub.update('Narasimha', blueprintConfig, inputcontrol);

    this.gtFormStatus = {
        submitted: false,
        checkOnSubmit: true,
        checkOnDirty: false,
        checkOnTouch: false,
        formName: 'MyForm'
        onControlUpdate:function(this: any, control: any){
            // console.log(control, this)
         }.bind(this)
    }

    /* .html implementation */
    < div [GtFormStatus]= "gtFormStatus" [templates]="{testtemplate: templatename}" [GtFormHub]="gtformhub">
        < gt-reactive-form 
            [control]="inputcontrol"
        < /gt-reactive-form>
    < /div>
    

## B1.2) Reactive Form component

    // .ts implementation

    import  { 
        GtFormHub, 
        sampleGtControlBluePrint, 
        sampleGtGroupBluePrint,
        sampleGtArrayBluePrint,
        sampleGtReactiveBluePrint, 
        GtControlBP, GtGroupBP, GtArrayBP 
    } from 'goteti-forms';

    export class AppComponent implements OnInit{
        data: (GtControlBP | GtGroupBP | GtArrayBP)  = sampleGtReactiveBluePrint;
        formData;
        gtformhub = new GtFormHub({
                customValidator: (function(contrl: AbstractControl){
                    return {customValidator: true};
                }).bind(this),
                customAsyncValidator: (function(contrl: AbstractControl){
                    return Promise.resolve({customAsyncValidator: true});
                }).bind(this)
            });
        ngOnInit(){            
            this.formData =  this.gtformhub.build(this.data);
            /* Now every FormControl , FormArray, FormGroup in this.formData object are added with 'config' property which has the configurtions related to the fields */
        }

        

        inputListen = {
            change : function(ev) {
                console.log('inputListen',arguments)
            }
        }
    }


    // html implementation
    < div [GtFormStatus]= "gtFormStatus" [templates]="{testtemplate: templatename}" [GtFormHub]="gtformhub">
        < gt-reactive-form 
                [control]="formData"
                [listenInput]="inputListen"
                >< /gt-reactive-form ></ div >

## B1.2.1 ) Reactive form with 'commonInputTemplate' support.

By default "gt-reactive-form" uses "gt-ifield" component to render the generic input fields.
    
If you want to use customized generic input component like angular material instead of the default "gt-ifield" you can use the 'commonInputTemplate' feature.

.ts implementation is Same as below, only changes is the .html

Remember : "customisedCommonInputField" in the below example is the project level common input field which you have to create in your project that supports all input types like text, number, radio slider, etc..

    // html implementation
    < div [GtFormStatus]= "gtFormStatus" [templates]="{commonInputTemplate:ownGenericInput}" [GtFormHub]="gtformhub">
        < gt-reactive-form 
                [control]="formData"                
        >< /gt-reactive-form> 
    < /div>

    < ng-template [gtref]="customField" let-config="config"  let-formStatus="formStatus" let-listen="listen" let-args="args" let-valueModel="valueModel" let-control="control" let-disabled="disabled"  let-all>
        < input [(ngModel)]="valueModel.value" [atbr]="config.atbr" [random]="control?.random"/>
    < /ng-template>

    < ng-template #ownGenericInput let-con="control" let-all>
        < customisedCommonInputField [control]="con"></ customisedCommonInputField>
    < /ng-template>


example of customisedCommonInputField from above example.

    //customisedCommonInputField.ts

    < ng-container [ngSwitch]="control?.config?.type">
        < ng-template *ngSwitchCase="'input'">
            Mat input component
        < /ng-template>
        < ng-template *ngSwitchCase="'slider'">
            Mat slider component
        < /ng-template>
        < ng-template *ngSwitchCase="'dropdown'">
            Mat dropdown component
        < /ng-template>
    < /ng-container>

###  Reactive form configuration object examples:
    FormControlDataExample = {
        isObject: false,
        isArray: false,
        skip: false,
        id: 'city',
        name: 'city',
        key: 'city',
        label: 'City',
        value: 'Hyderabad',
        type: 'text',
        disable: false,
        hide: false,
        atbr: {
            tabindex: 1
        },
        order: 3 ,
        divclass: 'col-md-3', // wrapper around the field
        class:'form-control',
        validations: [{
            key: 'required'
            asyncfn: false
        }, {
            key: 'minLength',
            asyncfn: false,
            value: 10
        },{
            key: 'customValidator',
            asyncfn: false,
            value: 'testvalue'
        },{
            key: 'customAsyncValidator',
            asyncfn: true,
            value: 'testvalue'
        }],
        errors :{
            required: {
                msg: 'First Name is required'
            },
            customValidator:{
                msg: 'Custom Validator Error Message'
            },
            customAsyncValidator:{
                msg: 'Custom Async Validator Error Message'
            }
        }
    }


    FormGroupDataExample = {
        isObject: true,
        isArray: false,
        init: 2,
        id: 'PermanentAddress',
        label: 'Permanent Address',
        class: 'col-md-9',
        objectClass: '', /* wrapper div class */
        order: 4,
        disable: false,
        hide: false,
        skip: false,
        fields:[{...FormControlDataExample}], /* FormControlDataExample or FormGroupDataExample*/
        validations: [],
        errors: {}       
    }

    FormArrayDataExample = {
        isArray: true
        isObject: false,
        id: 'Address',
        class: 'col-md-9',
        objectClass: '', /* wrapper object div class around each iteration.*/
        order: 4,
        disable: false,
        hide: false,
        skip: false,
        prop: {...FormGroupDataExample }, /* FormGroupDataExample or FormControlDataExample */
        validations: [{
            key: 'rangelimit', /* custome validator */
            value: 3,
            asyncfn: false
        }],
        errors: {
            rangelimit: {
                msg : 'Minimum 3 items are required',
                class: 'text-danger'
            }
        }
    }

# Have a look into :
> import  { sampleGtArrayBluePrint, sampleGtControlBluePrint, sampleGtGroupBluePrint, sampleGtReactiveBluePrint } from 'goteti-forms'; 

> import { GtArrayBP, GtControlBP, GtGroupBP, GtValidationBP, GtControlOptions } from 'goteti-forms';


| Properties| Description| Values Supported / Example | 
|-|-|-|
|isObject| true / false , Implementation of Formbuilder.Object, If isObject is true , "fields" property is mandatory. | let formhub = new GtFormHub({}); <br> let formdata = formhub.build(FormGroupDataExample);|
|isArray| true/false , Implementation of Formbuilder.Array. 'prop' is mandatory.| let formhub = new GtFormHub({}); <br> let formdata = formhub.build(FormArrayDataExample) |
|skip| (Boolean) Skips  the rendering of formControl/formGroup/formArray in <b>'gt-reactive-form'</b>| skip: true,/* skips rendering of control in gt-reactive-form **/ <br> skip: false, /* Renders the control in gt-reactive-form without skipping **/ |
|hide| (Boolean) Hides the control| hide: true,<br>hide: false|
|disable|(Boolean) Disables the control| disable: true,<br> disable: false|
|init| For arrays default add the no: of init number records| init: 3|
|prop| If isArray = true , then prop is mandatory which can be either 'FormGroupDataExample' or 'FormControlDataExample' |Same like above|
|actions|Removed support for actions in 'gt-reactive-form', please Refer B1.2.2 | Instead use {arrayIterateTemplate, postArrayTemplate, groupIterateTemplate, postGroupTemplate, postInputTemplate} |
|validations|Array of validations objects, if array or object needs validations. 'value' property should not be added for singleton validators like 'required'.<br>'asyncfn' is true for async validators <br> 'asyncfn' is false for normal validators|[{"key": "rangelimit","value": 3, asyncfn: false}]|
|errors|key and value Object for validation messages.|<pre>{"errors":{  "rangeCheck":{"msg": "Mininum 2 items are required","class": "text-danger"}}}</pre>|
|class, divclass, objectClasss| wrapper div classes| |
|inputs|acts like @Inputs for component if type:"component" and component:"< componentToLoad >"| {< key >: < value >}|
||||

<b>[control]:</b>
Form builder object.

<b>GtFormHub:</b>
It is Form builder generator, constructor accepts object with list of Validators and Async Validators as parameter.


Using 'gt-reactive-form' component will render the all the elements dynamically.


For explanation please find the properties of 'sampleReactiveFormData' i.e console.log(sampleReactiveFormData). This document update is still in-progress to list out the all the config properties of 'goteti-recursive-form-control'.


## B1.2.2 Alternative to actions in 'gt-reactive-form'
    < div [GtFormStatus]= "gtFormStatus" [templates]="{
                commonInputTemplate: ownGenericInput,
                arrayIterateTemplate: arrayIterateTemplate,
                postArrayTemplate: postArrayTemplate,
                groupIterateTemplate: groupIterateTemplate,
                postGroupTemplate: postGroupTemplate,
                postInputTemplate: postInputTemplate
            }" [GtFormHub]="gtformhub">

    <gt-reactive-form 
            [control]="formData"
            [listenInput]="inputListen"
            
    ></gt-reactive-form> 
    </ div>

    <ng-template gtref="ownGenericInput" let-con="control" let-all>
        <customisedCommonInputField [control]="con"></customisedCommonInputField>
        /* console.log(all) to view all properties available*/
    </ng-template>

    <ng-template #arrayIterateTemplate let-con="control" let-all>
       /* Place your actions here */
       /* console.log(all) to view all properties available*/
    </ng-template>

    <ng-template #groupIterateTemplate let-con="control" let-all>
        /* Place your actions here */
        /* console.log(all) to view all properties available*/
    </ng-template>

    <ng-template #postGroupTemplate let-con="control" let-all>
        /* Place your actions here */
        /* console.log(all) to view all properties available*/
    </ng-template>

    <ng-template #postInputTemplate let-con="control" let-all>
        /* Place your actions here */
        /* console.log(all) to view all properties available*/
    </ng-template>


## B1.2.3 'gt-error-msg' component : GtErrorMsgComponent

    <gt-error-msg [errors]="control?.errors" [errormessages]="control?.config?.errors" [control]="control"></gt-error-msg>

## B1.2.4 Added new properties to GtControl , GtGroup, GtArray (instances)

        1. random - String - Generates random String value when fireRandom or updateAtbr functions are triggered, which can be used as setter inbounds to the component. 
            ex: <custom-element [random]="control.random"><custom-element >
                @Input() 
                set random(){
                    this.callTheRefreshFunctionalityOrUpdateFunctionality()
                }

        2. fireRandom() - Function - Generates the random string value and assigns to random property of object.

        3. updateAtbr(atbr: GtAttributeBP) - Function - Updates the attribute property in 'control.config.atbr' object and fires the fireRandom()

        4. updateValidatorsList(validations: Array<GtValidationBP>, gtFormhubInstance: GtFormHub, checkValidation ?: boolean) - Function - Sets the new sync and async validators to the existing GtFormControl, GtFormGroup, GtFormArray instance objects using GtFormHub instance.

                ex: (this.firstname as GtControl).updateValidatorsList([{key: 'required'}, {key:'customasyncvalidator': asyncfn: true}],  this.formhub)

        5. insertToArray(index:Number,GtFormHub_Instance?: any, prop?: GtControlBP | GtGroupBP ) - Function  - Inserts the new GtControl / GtGroup to the GtArray at the specific index using GtFormHub instance.

        6. pushToArray(GtFormHub_Instance?: any, prop?: GtControlBP | GtGroupBP) - Function - Pushes the new GtControl / GtGroup to the GtArray at the end using GtFormHub instance.

        NOTE: 
            GtControl : random, fireRandom() , updateAtbr(), updateValidatorsList()
            GtGroup : random, fireRandom() ,  updateValidatorsList()
            GtArray : random, fireRandom() ,  updateValidatorsList(),  insertToArray(),  pushToArray()


## B1.3) Pipe: ItpolatePipe
Example:

    this.value = {
        name: 'India'
    }


    <span> {{ value | itpolate: 'Country name is {name}'  }} </span>




## B1.4) Directive: AtbrDirective , depends on [random]
Example:

    this.attributes = {
        step: 3,
        placeholder: 'This is the place holder'
    };

    this.random = Math.random();


    <input 
        [atbr]="attributes"
        [random]="random"
    />


## B1.5) Directive: [hide] , [disable]

    this.rules = {
        hide: true,
        disable: true
    }

    <input 
        *hide="rules.hide"
        [disable]="rules.disable"
    />


[disable]="true" , Disables the element even you change the properties in developer tools i.e inspect elements.



## B1.6) Directive : [listener]  (ListenerDirective)

    onInputChange(event){
        // some functionality
    }

    onKeypress(event){
        // some functionality
    }

    <input 
        [listener]="{input: onInputChange, keypress: onKeypress}"
        [args]="['Parameter 1', 'Parameter 2']"
    />


## B1.7 Directive: [gtLoadTemplate] : <ng-template [gtLoadTemplate]="< templateNameString >"></ng-template>
   Inputs:  config, listen, args, control, valueModel, templates, random, gtLoadTemplate(v: string);

    Note: This directive load the template from GtFormStatusService or else GtFormService by searching the template string gtLoadTemplate property


## B1.8 Directive [gtLoadComponent] : <ng-template [gtLoadComponent]="< componentNameString >"></ng-template>
    Inputs:  config, listen, args, control, valueModel, random(r: string, gtLoadComponent(v: string)

    Note: This directive load the component from GtFormStatusService or else GtFormService by searching the component string gtLoadComponent property

## B1.9 Directive [gtref]

    <ng-template gtref="CustomeTemplateName" let-valueModel="valueModel" let-control="control" let-config="config" let-formStatus="formStatus" let-listen="listen" let-args="args">
        <input [formControl]="control" [atbr]="config?.atbr" [random]="control?.random"/> 

                    (OR)

        <input [(ngModel)]="valueModel.value" [atbr]="config?.atbr" [random]="control?.random"/> 
    </ng-template>

    Note: This directive will add this template to the current scope of GtFormService








## Licence

Open Source. 

<small>The author is NOT liable for any liabilities.</small>
