# reconcile-ractive-mvp

POC regarding ractive and MVP.

```
var View = function(service){
  var view = new Ractive({
    el: '#output',
    template: '#template',
    data: {
      divData: 'data for div'
    },
    updateWithDataFromService: function(data){
      this.set('divData', data);
    }
  });
  new Presenter(service, view);
  return view;
};

var Presenter = function(service, view){
  this.service = service;
  view.on('activate', service.handleSomething.bind(service));
  service.commandFromService = view.updateWithDataFromService.bind(view);
};

var Service = function(){};
Service.prototype.handleSomething = function(){
  alert('service called');
};

var service = new Service();
var view = new View(service);

window.setTimeout(function(){
  service.commandFromService('some data from the service');
}, 1000);
```
