
olean.valueOf(vars.get("DEBUG"))) {
  for (a: SampleResult.getAssertionResults()) {
      if (a.isError() || a.isFailure()) {
            log.error(Thread.currentThread().getName()+": "+SampleLabel+": Assertion failed for response: " + new String((byte[]) ResponseData));
	        }
		  }
		  }



		  if (Boolean.valueOf(vars.get("DEBUG"))) {
		    for (a: SampleResult.getAssertionResults()) {
		        if (a.isError() || a.isFailure()) {
			      log.error(Thread.currentThread().getName()+": "+SampleLabel+": Assertion failed for response: " + new String((byte[]) ResponseData));
			          }
				    }
				    }
