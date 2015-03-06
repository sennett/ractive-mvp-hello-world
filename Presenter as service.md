Here the presenter is a service provided by IOC.  It constructs the view and binds the events.

Main weakness is the presenter cannot be used by a bunch of different views.

	var baseViewDef = {
	  el: '#output',
	  template: '#template',
	  data: {
	    divData: 'data for div'
	  },
	  updateWithDataFromService: function(data){
	    this.set('divData', data);
	  }
	};

	var Presenter = function(service){
	  this.service = service;
	};

	Presenter.prototype.getView = function(){
	  var viewDef = Object.create(baseViewDef);
	  
	  var view = new Ractive(viewDef);
	  
	  view.on('activate', this.service.handleSomething.bind(this.service));
	  this.service.commandFromService = viewDef.updateWithDataFromService.bind(view);
	};

	var Service = function(){};
	Service.prototype.handleSomething = function(){
	  alert('service called');
	};

	var service = new Service();
	new Presenter(service).getView();
	window.setTimeout(function(){
	  service.commandFromService('some data from the service');
	}, 1000);
